---
title: 使用SSH包装Socks5代理
date: 2023-11-22 17:38:25
categories: 
- 网络编程
tags:
- 网络编程
- SSH
---

# subSocks简介

[subSocks](https://github.com/luyuhuang/subsocks)是[Luyu Hang](https://luyuhuang.tech/)制作的纯golang网络代理软件。

这里是作者本人对此项目的介绍[文档](https://luyuhuang.tech/2020/12/02/subsocks.html)。

<!-- more -->

# 为什么要做SSH包装

因为之前使用v2ray总是被封端口，但是VPS上的22端口始终建在，考虑到SSH协议比较复杂，包括了Shell，SFTP等多种应用。我认为使用SSH协议包装流量可以起到一定的伪装作用，减少端口被封的可能性。

subSocks项目的代码结构非常漂亮，添加SSH包装非常便捷。

# 实现过程

首先需要了解subSocks的代码结构，Luyu Hang的[文档](https://luyuhuang.tech/2020/12/02/subsocks.html)中描述的非常详细，我只需要实现SSHWarpper和SSHStripper。

golang已经提供了SSH的官方实现，参考[文档](https://pkg.go.dev/golang.org/x/crypto/ssh)。并且提供了使用SSH进行远程Shell的示例。

之后需要对SSH的[通讯过程](/2023/11/20/SSH.html)，```Session``` ```Channel``` ```Request```等等各种概念有基础的了解。

使用ssh包中的代码，在服务端使用TCP链接，创建SSH服务器，等待客户端链接后获取Channel，将Channel包装为Stripper。

客户端与服务端相似，需要使用TCP链接，向服务端完成握手过程，之后可获得Session，将Session包装成Wrapper。

# 使用

服务端必须配置密钥，可使用自己生成的密钥：

``` toml toml
[server] # server configuration

protocol = "ssh"
listen = "0.0.0.0:22"

ssh.cert = "./id_rsa.pub"
ssh.key = "./id_rsa"
```

客户端只需将协议设置为ssh，其他与subsocks相同:

``` toml toml
[client] # client configuration

listen = "127.0.0.1:1080"

server.protocol = "ssh"
server.address = "serverIP:22"
```

通过抓包验证，握手过程正常，通讯过程与SSH相同，多条链接使用正常，所有数据均经过加密：

![](wireshark.png)

通过观看视频网站验证，视频加载流畅，体验很好。