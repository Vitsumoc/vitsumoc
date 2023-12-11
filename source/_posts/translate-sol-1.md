---
title: "[翻译]Sol - 从零开始的MQTT broker - 第一部分：协议"
date: 2023-12-08 15:30:15
categories:
- 网络编程
tags:
- 网络编程
- 翻译
---

> 原文 [Sol - An MQTT broker from scratch. Part 1 - The protocol](https://codepr.github.io/posts/sol-mqtt-broker/)

我已经在物联网领域工作有一段时间了，这段时间里我一直在处理物联网架构相关的工作，探索物联网系统开发的最佳模式，研究相关的协议和标准，例如MQTT。

<!--more-->

因为我一直在渴望着提升我编程能力的机会，我觉得在物联网方向深入研究会很有趣也很有好处。因此，我再一次 `git init` 了一个项目，并且要通过写下这些博客来挑战我自己，让我自己更上一层楼。

**Sol** 是一个C语言项目，一个超级简单的Linux平台的MQTT broker，支持MQTT 3.3.1，不兼容旧的版本，非常类似于轻量级的 `Mosquitto` （虽然这玩意已经是个轻量级软件了）。由于现在有很多种类的MQTT客户端，所以测试起来会比较简单。最终的结果可能会成为一个更简洁，功能更丰富的软件，我们要创造这个功能的最小化实现。顺便提一下，**Sol** 这个名字的来源有一半的原因是我对短名称的偏好，另一半的原因则是火星日 (The Martian docet)。或者说，**Sol** 可能代表**S**crappy **O**l' **L**oser。emmmm

**注意**：这个项目一直到最后才会编译，你需要跟写所有的代码步骤。如果你想要进行模块测试，我建议你自己建一个主函数来做这些测试或者修改。

一步一步来，我一般会创建一个这样的文件结构来初始化我的C项目：

```
sol/
 ├── src/
 ├── CHANGELOG
 ├── CMakeLists.txt
 ├── COPYING
 └── README.md
```

这里是Github上的[仓库](https://github.com/codepr/sol/tree/tutorial)。

我会尝试着一步一步描述 **Sol** 的开发过程，但我也不会贴上所有的代码，只会解释关键的地方。你想要学习的最好方式依然是亲自编写、编译、修改代码。

这将是一系列文章，每篇文章都将讨论并主要实施项目的一个概念/模块：

- [第一部分 ： 协议](https://codepr.github.io/posts/sol-mqtt-broker/) MQTT协议数据包处理的基础
- [第二部分 ： 网络](https://codepr.github.io/posts/sol-mqtt-broker-p2/) 解决网络通讯的功能模块
- [第三部分 ： 服务](https://codepr.github.io/posts/sol-mqtt-broker-p3/) 程序入口
- [第四部分 ： 数据结构](https://codepr.github.io/posts/sol-mqtt-broker-p4/) 实用和实验性的模块
- [第五部分 ： Topic abstraction](https://codepr.github.io/posts/sol-mqtt-broker-p5/) broker的主要功能
- [第六部分 ： Handlers](https://codepr.github.io/posts/sol-mqtt-broker-p6/) 完成服务，对每个数据包都有处理
- [特别篇 ： 多线程](https://codepr.github.io/posts/sol-mqtt-broker-bonus/) 多线程模型的改进、错误修复和集成

我想强调的是，最终的软件将是一个完全功能的代理，但仍有很大改进和优化空间，以及代码质量改进，以及可能的一些隐藏功能。（俗称BUG =。=）

### 通用架构

`broker` 的本质是一个中间件，它接受来自多个客户端（生产者）的输入，并使用抽象方法将其转发给一组目标客户端（消费者），这种抽象方法用于定义和管理这些客户端组，形式为 **channel** 或 **topic**（根据协议标准）。与 IRC 频道或通用聊天中的等效概念非常相似，每个消费者客户端都可以订阅 `topic`，以便接收其他客户端发布到这些 `topic` 的所有消息。

第一个想到的是建立在某种数据结构之上的服务器，这种数据结构可以轻松管理这些 `topic` 和连接的客户端（无论是生产者还是消费者）。客户端收到的每个消息都必须转发给所有订阅了该消息指定 `topic` 的其他已连接客户端。

让我们试试这种方法，使用一个 TCP 服务器和一个用于处理二进制通信的模块。实现服务器有很多方法，包括线程、fork 进程和多路 I/O，我将尝试用多路 I/O 的方式。

我们先使用单线程多路 I/O 服务器，未来有可能进行多线程拓展。实际上，用于多路复用的 **epoll** 接口是线程安全的。

### MQTT 协议