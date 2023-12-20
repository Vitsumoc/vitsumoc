---
title: "[翻译]Sol - 从零开始的MQTT broker - 第二部分：网络"
date: 2023-12-19 17:13:46
categories:
- 网络编程
tags:
- 网络编程
- 物联网
- 翻译
---

> 原文 [Sol - An MQTT broker from scratch. Part 2 - Networking](https://codepr.github.io/posts/sol-mqtt-broker-p2/)

# 前言

让我们继续之前的工作，在第一部分中我们实现了 MQTT v3.1.1 的数据结构和解码函数，接下来我们需要做一些组包和编码函数，让我们可以发送网络包。

<!--more-->

顺带说明一下，我们并没有打算去编写完美的或者内存效率很高的代码，而且，过早的优化是万恶之源，以后我们有的是时间来提高我们的代码质量。

# 组包实现

暂时我们只需要做 `CONNACK` `SUBACK` `PUBLISH` 包的组包工作，其他的各种 `ACK` 的结构都是一样的，之前我们已经用 **typedef** 让这些 `ACK` 引用了同一个函数。

- `union mqtt_header *mqtt_packet_header(unsigned char)` 函数用来处理 Fixed Header，以及以下这些只有 Fixed Header 的包：
  - PINGREQ
  - PINGRESP
  - DISCONNECT

- `struct mqtt_ack *mqtt_packet_ack(unsigned char, unsigned short)` 用来处理以下这些 `类ACK` 的包：
  - PUBACK
  - PUBREC
  - PUBREL
  - PUBCOMP
  - UNSUBACK

其余的包都需要专门的函数来组包。再说一次，虽然可能有很多更优雅的代码或者更优化的方法，但是现在我们只要写能用的代码就行了，以后迟早会优化的。

```c mqtt.c
/*
 * mqtt组包
 */

// 头部1byte的组包实现
union mqtt_header *mqtt_packet_header(unsigned char byte) {
    static union mqtt_header header;
    header.byte = byte;
    return &header;
}

// 各种ACK的组包实现
struct mqtt_ack *mqtt_packet_ack(unsigned char byte, unsigned short pkt_id) {
    static struct mqtt_ack ack;
    ack.header.byte = byte;
    ack.pkt_id = pkt_id;
    return &ack;
}

// CONNACK 组包实现
struct mqtt_connack *mqtt_packet_connack(unsigned char byte,
                                         unsigned char cflags,
                                         unsigned char rc) {
    static struct mqtt_connack connack;
    connack.header.byte = byte;
    connack.byte = cflags;
    connack.rc = rc;
    return &connack;
}

// SUBACK 组包实现
struct mqtt_suback *mqtt_packet_suback(unsigned char byte,
                                       unsigned short pkt_id,
                                       unsigned char *rcs,
                                       unsigned short rcslen) {
    struct mqtt_suback *suback = malloc(sizeof(*suback));
    suback->header.byte = byte;
    suback->pkt_id = pkt_id;
    suback->rcslen = rcslen;
    suback->rcs = malloc(rcslen);
    memcpy(suback->rcs, rcs, rcslen);
    return suback;
}

// PUBLISH 组包实现
struct mqtt_publish *mqtt_packet_publish(unsigned char byte,
                                         unsigned short pkt_id,
                                         size_t topiclen,
                                         unsigned char *topic,
                                         size_t payloadlen,
                                         unsigned char *payload) {
    struct mqtt_publish *publish = malloc(sizeof(*publish));
    publish->header.byte = byte;
    publish->pkt_id = pkt_id;
    publish->topiclen = topiclen;
    publish->topic = topic;
    publish->payloadlen = payloadlen;
    publish->payload = payload;
    return publish;
}

// 释放包资源
void mqtt_packet_release(union mqtt_packet *pkt, unsigned type) {
    switch (type) {
        case CONNECT:
            free(pkt->connect.payload.client_id);
            if (pkt->connect.bits.username == 1)
                free(pkt->connect.payload.username);
            if (pkt->connect.bits.password == 1)
                free(pkt->connect.payload.password);
            if (pkt->connect.bits.will == 1) {
                free(pkt->connect.payload.will_message);
                free(pkt->connect.payload.will_topic);
            }
            break;
        case SUBSCRIBE:
        case UNSUBSCRIBE:
            for (unsigned i = 0; i < pkt->subscribe.tuples_len; i++)
                free(pkt->subscribe.tuples[i].topic);
            free(pkt->subscribe.tuples);
            break;
        case SUBACK:
            free(pkt->suback.rcs);
            break;
        case PUBLISH:
            free(pkt->publish.topic);
            free(pkt->publish.payload);
            break;
        default:
            break;
    }
}
```

# 编码实现

我们接下来处理编码函数，编码函数其实就是解码函数的反方向操作：我们使用内存对象创造一个字节流，之后可以通过socket发出去。

现在我们有一些函数返回指向 `static struct` 的指针（例如上方代码中的 `mqtt_packet_header` ），在单线程的情况下这是没什么问题的。 **在多线程环境下，一定会出问题**，每次这种函数的返回都会指向同一片内存区域，可能导致各种冲突。因此为了将来的改进，需要重构这些部分，使用 `malloc` 来为每次返回分配地址。

我们采用和之前解码函数一样的方式来映射编码函数。做一个静态数组，其中的序号恰好等于包类型。为了让源代码更加简洁，我们可以将打包和解包处理程序分组到一个结构中，因此可以使用单个数组，因为它们共享相同的位置。

```c src/mqtt.c

// MQTT 编码函数接口
typedef unsigned char *mqtt_pack_handler(const union mqtt_packet *);

// 编码函数数组, 其中索引和包类型id对应
static mqtt_pack_handler *pack_handlers[13] = {
    NULL,
    NULL,
    pack_mqtt_connack,
    pack_mqtt_publish,
    pack_mqtt_ack,
    pack_mqtt_ack,
    pack_mqtt_ack,
    pack_mqtt_ack,
    NULL,
    pack_mqtt_suback,
    NULL,
    pack_mqtt_ack,
    NULL
};

// header 的编码实现
static unsigned char *pack_mqtt_header(const union mqtt_header *hdr) {
    unsigned char *packed = malloc(MQTT_HEADER_LEN);
    unsigned char *ptr = packed;
    pack_u8(&ptr, hdr->byte);
    // Remaining Length 1byte 值为0
    mqtt_encode_length(ptr, 0);
    return packed;
}

// ACK 的编码实现
static unsigned char *pack_mqtt_ack(const union mqtt_packet *pkt) {
    unsigned char *packed = malloc(MQTT_ACK_LEN); // 4byte
    unsigned char *ptr = packed;
    pack_u8(&ptr, pkt->ack.header.byte);
    mqtt_encode_length(ptr, MQTT_HEADER_LEN); // 这里指还有2byte 内容是 pkt_id
    ptr++; // 因为 mqtt_encode_length 不会移动指针, 只会返回 Remaining Length 的长度, 而这里长度显然为1
    pack_u16(&ptr, pkt->ack.pkt_id);
    return packed;
}

// CONNACK 的编码实现
static unsigned char *pack_mqtt_connack(const union mqtt_packet *pkt) {
    unsigned char *packed = malloc(MQTT_ACK_LEN);
    unsigned char *ptr = packed;
    pack_u8(&ptr, pkt->connack.header.byte);
    mqtt_encode_length(ptr, MQTT_HEADER_LEN);
    ptr++;
    pack_u8(&ptr, pkt->connack.byte);
    pack_u8(&ptr, pkt->connack.rc);
    return packed;
}

// SUBACK 的编码实现
static unsigned char *pack_mqtt_suback(const union mqtt_packet *pkt) {
    // 计算总长度
    size_t pktlen = MQTT_HEADER_LEN + sizeof(uint16_t) + pkt->suback.rcslen;
    unsigned char *packed = malloc(pktlen + 0);
    unsigned char *ptr = packed;
    // 编码固定头
    pack_u8(&ptr, pkt->suback.header.byte);
    // 剩余部分的长度
    size_t len = sizeof(uint16_t) + pkt->suback.rcslen;
    // 变长表示剩余部分长度
    int step = mqtt_encode_length(ptr, len);
    // 指针后移
    ptr += step;
    // 剩余部分编码
    pack_u16(&ptr, pkt->suback.pkt_id);
    for (int i = 0; i < pkt->suback.rcslen; i++)
        pack_u8(&ptr, pkt->suback.rcs[i]);
    return packed;
}

// PUBLISH 的编码实现
static unsigned char *pack_mqtt_publish(const union mqtt_packet *pkt) {
    // pktlen 至少有这么多: 头部至少2byte(1byte头 + 至少1byte的Remaining Length)
    // sizeof(uint16_t) 表示 topiclen 的长度, 因为 payloadlen 是不被编码到字节流中的
    // topiclen 和 payloadlen 的内容
    size_t pktlen = MQTT_HEADER_LEN + sizeof(uint16_t) +
        pkt->publish.topiclen + pkt->publish.payloadlen;
    // 这里是去除 fixed header 之外的内容长度
    size_t len = 0L;
    // qos > 0, 说明有pkt_id, 需要 +2byte
    if (pkt->header.bits.qos > AT_MOST_ONCE)
        pktlen += sizeof(uint16_t);
    // 这里是通过剩余长度计算变长部分还需要的长度, 前面已经预留了1byte
    int remaininglen_offset = 0;
    if ((pktlen - 1) > 0x200000)
        remaininglen_offset = 3;
    else if ((pktlen - 1) > 0x4000)
        remaininglen_offset = 2;
    else if ((pktlen - 1) > 0x80)
        remaininglen_offset = 1;
    // 这里是总包长
    pktlen += remaininglen_offset;
    unsigned char *packed = malloc(pktlen);
    unsigned char *ptr = packed;
    pack_u8(&ptr, pkt->publish.header.byte);
    // 除去 fixed header 之外剩余部分的长度
    len += (pktlen - MQTT_HEADER_LEN - remaininglen_offset);
    // 编码 Remaining Length
    int step = mqtt_encode_length(ptr, len);
    ptr += step;
    // 编码 topiclen 和后续的 topic 内容
    pack_u16(&ptr, pkt->publish.topiclen);
    pack_bytes(&ptr, pkt->publish.topic);
    // 当 QoS > 0 时, 编码 pkt_id
    if (pkt->header.bits.qos > AT_MOST_ONCE)
        pack_u16(&ptr, pkt->publish.pkt_id);
    // 编码 payload 的内容
    pack_bytes(&ptr, pkt->publish.payload);
    return packed;
}

// 编码函数入口
unsigned char *pack_mqtt_packet(const union mqtt_packet *pkt, unsigned type) {
    if (type == PINGREQ || type == PINGRESP)
        return pack_mqtt_header(&pkt->header);
    return pack_handlers[type](pkt);
}
```

# 服务器

我们计划创建一个单线程 TCP 服务器，使用 **epoll** 接口实现多路 I/O。Epoll 是继 **select** 和 **poll** 之后内核 2.5.44 添加的最新的多路复用机制，也是性能最高、连接数最多的多路复用机制，它在 BSD 和 BSD-like (Mac OSX) 系统中的对应机制是 **kqueue**。

我们需要定义一些函数来管理我们的socket descriptor。

```c src/network.h
#include <stdio.h>
#include <stdint.h>
#include <sys/types.h>
#include "util.h"

// 地址族
#define UNIX    0
#define INET    1

/* Set non-blocking socket */
int set_nonblocking(int);

/*
 * Set TCP_NODELAY flag to true, disabling Nagle's algorithm, no more waiting
 * for incoming packets on the buffer
 */
int set_tcp_nodelay(int);

// 创建 epoll 服务器的辅助函数
int create_and_bind(const char *, const char *, int);

// 创建一个 non-blocking socket 并监听指定的地址和端口
int make_listen(const char *, const char *, int);

// 接收链接并进行后续处理, 将链接分配到 epollfd
int accept_connection(int);
```

定义了一些简单的辅助函数，用来创建和绑定 `socket` 端口，处理新链接并把 `socket` 设置为 `non-blocking` 模式（这样才能发挥 **epoll** 的复用能力）。

我不喜欢必须处理所有进出服务器的字节流，在我写的涉及到TCP通信的程序中，我都会定义这两个函数：
- `ssize_t send_bytes(int, const unsigned char *, size_t)` 用于在while循环中持续发送数据，直到把数据全部发送完。正确捕获 `EAGAIN` 或 `EWOUDLBLOCK` 异常。
- `ssize_t recv_bytes(int, unsigned char *, size_t)` 在while循环中获得任意长度的数据。正确捕获 `EAGAIN` 或 `EWOUDLBLOCK` 异常。

```c src/network.h
/* I/O management functions */

/*
 * Send all data in a loop, avoiding interruption based on the kernel buffer
 * availability
 */
ssize_t send_bytes(int, const unsigned char *, size_t);

/*
 * Receive (read) an arbitrary number of bytes from a file descriptor and
 * store them in a buffer
 */
ssize_t recv_bytes(int, unsigned char *, size_t);
```

接下来是 `network.c` 的实现。

```c src/network.c
#define _DEFAULT_SOURCE
#include <stdlib.h>
#include <errno.h>
#include <netdb.h>
#include <unistd.h>
#include <fcntl.h>
#include <arpa/inet.h>
#include <sys/un.h>
#include <sys/epoll.h>
#include <sys/timerfd.h>
#include <netinet/in.h>
#include <netinet/tcp.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/eventfd.h>
#include "network.h"
#include "config.h"

// 设置 non-blocking socket
int set_nonblocking(int fd) {
    int flags, result;
    flags = fcntl(fd, F_GETFL, 0);
    if (flags == -1)
        goto err;
    result = fcntl(fd, F_SETFL, flags | O_NONBLOCK);
    if (result == -1)
        goto err;
    return 0;
err:
    perror("set_nonblocking");
    return -1;
}

// 设置 TCP_NODELAY 用以关闭 Nagle's algorithm
int set_tcp_nodelay(int fd) {
    return setsockopt(fd, IPPROTO_TCP, TCP_NODELAY, &(int) {1}, sizeof(int));
}

// UNIX socket 的绑定方法
// return fd
// sockpath 文件路径
static int create_and_bind_unix(const char *sockpath) {
    struct sockaddr_un addr;
    int fd;
    // 创建 socket
    if ((fd = socket(AF_UNIX, SOCK_STREAM, 0)) == -1) {
        perror("socket error");
        return -1;
    }
    // addr初始值全0
    memset(&addr, 0, sizeof(addr));
    // 赋值
    addr.sun_family = AF_UNIX;
    strncpy(addr.sun_path, sockpath, sizeof(addr.sun_path) - 1);

    // 译者没有明白为何 unlink 会出现在此处
    unlink(sockpath);

    // 绑定 socket
    if (bind(fd, (struct sockaddr*) &addr, sizeof(addr)) == -1) {
        perror("bind error");
        return -1;
    }
    return fd;
}

// TCP socket 的绑定方法
// return fd
// host TCP 地址
// port TCP 端口
static int create_and_bind_tcp(const char *host, const char *port) {
    struct addrinfo hints = {
        .ai_family = AF_UNSPEC,       // 不指定协议族, 系统自定可以是IP4 或 IP6
        .ai_socktype = SOCK_STREAM,   // 面向流, 就是TCP
        .ai_flags = AI_PASSIVE        // 被动模式, 可以监听任意地址端口
    };
    // result 是 getaddrinfo 提供的 addrinfo, rp 指如果绑定不成功, 可以变成下一个 addrinfo
    struct addrinfo *result, *rp;
    int sfd;
    if (getaddrinfo(host, port, &hints, &result) != 0) {
        perror("getaddrinfo error");
        return -1;
    }
    for (rp = result; rp != NULL; rp = rp->ai_next) {
        // 先使用 rp 生成 socket
        sfd = socket(rp->ai_family, rp->ai_socktype, rp->ai_protocol);
        // 如果失败就下一个 rp
        if (sfd == -1) continue;
        // 设置 SO_REUSEADDR 这样关闭进程后可以重用端口
        if (setsockopt(sfd, SOL_SOCKET, SO_REUSEADDR,
                       &(int) { 1 }, sizeof(int)) < 0)
            perror("SO_REUSEADDR");
        if ((bind(sfd, rp->ai_addr, rp->ai_addrlen)) == 0) {
            // bind 成功
            break;
        }
        // 绑定失败记得关闭 socket
        close(sfd);
    }
    if (rp == NULL) {
        perror("Could not bind");
        return -1;
    }
    freeaddrinfo(result);
    return sfd;
}

// 绑定入口
int create_and_bind(const char *host, const char *port, int socket_family) {
    int fd;
    if (socket_family == UNIX)
        fd = create_and_bind_unix(host);
    else
        fd = create_and_bind_tcp(host, port);
    return fd;
}

// 创建一个 non-blocking socket, 监听指定的地址端口
// return sfd
// host 地址或UNIX path
// port 端口
// socket_family 地址族 AF_UNIX 或 AF_INET
int make_listen(const char *host, const char *port, int socket_family) {
    int sfd;
    if ((sfd = create_and_bind(host, port, socket_family)) == -1)
        abort();
    if ((set_nonblocking(sfd)) == -1)
        abort();
    // 仅当 TCP链接时设置 TCP_NODELAY
    if (socket_family == INET)
        set_tcp_nodelay(sfd);
    // conf是本程序的配置文件
    if ((listen(sfd, conf->tcp_backlog)) == -1) {
        perror("listen");
        abort();
    }
    return sfd;
}

// 接收链接后的处理
// return 客户端 fd
// serversock 服务端fd
int accept_connection(int serversock) {
    int clientsock;
    struct sockaddr_in addr;
    socklen_t addrlen = sizeof(addr);
    if ((clientsock = accept(serversock,
                             (struct sockaddr *) &addr, &addrlen)) < 0)
        return -1;
    set_nonblocking(clientsock);

    // 仅当 TCP链接时设置 TCP_NODELAY
    if (conf->socket_family == INET)
        set_tcp_nodelay(clientsock);
    char ip_buff[INET_ADDRSTRLEN + 1];
    // 将ip地址转为文本, 这里用作检查客户端地址
    if (inet_ntop(AF_INET, &addr.sin_addr,
                  ip_buff, sizeof(ip_buff)) == NULL) {
        close(clientsock);
        return -1;
    }
    return clientsock;
}

// 向 fd 发送指定长度的数据
// return 成功发送的数据长度
// fd 发送数据的目的
// buf 发送数据内容地址
// len 需要发送的数据长度
ssize_t send_bytes(int fd, const unsigned char *buf, size_t len) {
    // 发送数据的总长度
    size_t total = 0;
    // 剩余需要发送数据的长度
    size_t bytesleft = len;
    // 单次发送数据长度
    ssize_t n = 0;
    while (total < len) {
        // 发送 bytesleft 长度的数据
        n = send(fd, buf + total, bytesleft, MSG_NOSIGNAL);
        if (n == -1) {
            // 当 fd 被阻塞时, 直接返回已经发送的长度
            if (errno == EAGAIN || errno == EWOULDBLOCK)
                break;
            else
                goto err;
        }
        total += n;
        bytesleft -= n;
    }
    return total;
err:
    fprintf(stderr, "send(2) - error sending data: %s", strerror(errno));
    return -1;
}

// 从 fd 中获得指定长度的数据
// retrun 成功读取的长度 -1 表示异常
// fd 数据源
// buf 存放结果的指针
// bufsize 期望读取的数据长度
ssize_t recv_bytes(int fd, unsigned char *buf, size_t bufsize) {
    // 单次获取的数据长度
    ssize_t n = 0;
    // 获取的总数据长度
    ssize_t total = 0;
    while (total < (ssize_t) bufsize) {
        // 使用 recv 函数获得最大 bufsize - total 的数据
        if ((n = recv(fd, buf, bufsize - total, 0)) < 0) {
            // fd被阻塞了, 此时total的返回也许是小于 bufsize 的值, 调用者可以选择重试
            if (errno == EAGAIN || errno == EWOULDBLOCK) {
                break;
            } else
                // 对于其他的异常则报错
                goto err;
        }
        if (n == 0)
            return 0;
        buf += n;
        total += n;
    }
    return total;
err:
    fprintf(stderr, "recv(2) - error reading data: %s", strerror(errno));
    return -1;
}
```

# 基本封装

为了让 **epoll** 的使用更加简单和舒适，而且这个项目不需要那么复杂的操作。我在epoll基础上构建了一个简单的抽象，这样可以在事件点上注册回调函数。

网络上有很多使用 epoll 的示例，大部分都是描述基本用法：注册一个 socket 并启动一个循环来监听事件，每当 socket 需要被读写时，调用一个函数来使用它们。这些例子当然简单好用，但是也有一些局限性。我决定使用 `epoll_data`：

```c
typedef union epoll_data {
   void        *ptr;
   int          fd;
   uint32_t     u32;
   uint64_t     u64;
} epoll_data_t;
```

正如你看到的，

如图所示，有一个“void *”，一个常用于存储我们正在讨论的描述符的int和两个不同大小的整数。 我更喜欢使用带有描述符和其他一些上下文字段的自定义结构，特别是函数指针及其可选参数。 我们将注册一个指向该结构的指针，并将其传递给指针“void *ptr”。 这样，每次发生事件时，我们都可以访问我们注册的相同结构指针，包括关联的文件描述符。

As shown, there is a `void *`, an int commonly used to store the descriptor we were talking about and two integer of different size. I prefered to use a custom structure with the descriptor inside and some other context fields, specifically a function pointer and its optional arguments. We’ll register a pointer to this structure passing it to the pointer `void *ptr`. This way, every time an event occur, we’ll have access to the very same structure pointer we registered, including the file descriptor associated.

可以定义两种类型的回调，常见的回调将由事件触发，而周期性的回调将在定义的每个时间间隔自动执行。 因此，让我们将 epoll 循环包装到一个专用结构中，我们将对回调函数执行相同的操作，定义一个结构，其中包含一些对执行回调有用的字段。

There’s two types of callback which can be defined, the common ones, that will be triggered with events and the periodic ones, that will be executed automatically every tick of time interval defined. So let’s wrap the epoll loop into a dedicated structure, we’ll do the same for the callback functions, defining a structure with some fields useful for the execution of the callback.

**Sequential diagram, for each cycle of epoll_wait on incoming events**
![Epoll sequential diagram](epoll-sequential.png)

我们需要定义两个结构体和一个函数指针

We're going to declare two structures and a function pointer:

- **struct evloop** epoll 实例的封装，包括所有需要的参数
- **struct closure** 它抽象了一个回调和一种带有参数的上下文以及结果的序列化有效负载
- **void callback(struct evloop *, void *)** **closure**的内容，后续我们需要传递的回调函数接口

- **struct evloop**, a wrapper around the epoll instance, encapsulating all
  needed properties
- **struct closure** which abstract a callback and a sort of context with
  arguments and a serialized payload of the results
- **void callback(struct evloop *, void *)**, the heart of the **closure**, it's
  the prototype of the function we're gonna pass as callback.

另外，我们需要在 .c 文件中实现一些创建、删除和管理功能。

Plus, we'll declare and implement on the .c file some creation, delete and managing functions.

```c src/network.h
// 
/* Event loop wrapper structure, define an EPOLL loop and his status. The
 * EPOLL instance use EPOLLONESHOT for each event and must be re-armed
 * manually, in order to allow future uses on a multithreaded architecture.
 */
struct evloop {
    int epollfd;
    int max_events;
    int timeout;
    int status;
    struct epoll_event *events;
    /* Dynamic array of periodic tasks, a pair descriptor - closure */
    int periodic_maxsize;
    int periodic_nr;
    struct {
        int timerfd;
        struct closure *closure;
    } **periodic_tasks;
} evloop;

typedef void callback(struct evloop *, void *);

/*
 * Callback object, represents a callback function with an associated
 * descriptor if needed, args is a void pointer which can be a structure
 * pointing to callback parameters and closure_id is a UUID for the closure
 * itself.
 * The last two fields are payload, a serialized version of the result of
 * a callback, ready to be sent through wire and a function pointer to the
 * callback function to execute.
 */
struct closure {
    int fd;
    void *obj;
    void *args;
    char closure_id[UUID_LEN];
    struct bytestring *payload;
    callback *call;
};

struct evloop *evloop_create(int, int);
void evloop_init(struct evloop *, int, int);
void evloop_free(struct evloop *);

/*
 * Blocks in a while(1) loop awaiting for events to be raised on monitored
 * file descriptors and executing the paired callback previously registered
 */
int evloop_wait(struct evloop *);

/*
 * Register a closure with a function to be executed every time the
 * paired descriptor is re-armed.
 */
void evloop_add_callback(struct evloop *, struct closure *);

/*
 * Register a periodic closure with a function to be executed every
 * defined interval of time.
 */
void evloop_add_periodic_task(struct evloop *,
                              int,
                              unsigned long long,
                              struct closure *);

/*
 * Unregister a closure by removing the associated descriptor from the
 * EPOLL loop
 */
int evloop_del_callback(struct evloop *, struct closure *);

/*
 * Rearm the file descriptor associated with a closure for read action,
 * making the event loop to monitor the callback for reading events
 */
int evloop_rearm_callback_read(struct evloop *, struct closure *);

/*
 * Rearm the file descriptor associated with a closure for write action,
 * making the event loop to monitor the callback for writing events
 */
int evloop_rearm_callback_write(struct evloop *, struct closure *);

/* Epoll management functions */
int epoll_add(int, int, int, void *);

/*
 * Modify an epoll-monitored descriptor, automatically set EPOLLONESHOT in
 * addition to the other flags, which can be EPOLLIN for read and EPOLLOUT for
 * write
 */
int epoll_mod(int, int, int, void *);

/*
 * Remove a descriptor from an epoll descriptor, making it no-longer monitored
 * for events
 */
int epoll_del(int, int);
```