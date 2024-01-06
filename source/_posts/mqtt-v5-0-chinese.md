---
title: MQTT 5.0 中文文档(施工中)
date: 2024-01-06 10:14:39
categories:
- MQTT
tags:
- MQTT
- 网络协议
---

> 原文 [MQTT Version 5.0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html)
> **\[mqtt-v5.0]**
> MQTT Version 5.0. Edited by Andrew Banks, Ed Briggs, Ken Borgendale, and Rahul Gupta. 07 March 2019. OASIS Standard. https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html. Latest version: https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html.

<!-- more -->

# MQTT 5.0

OASIS 标准

2019年3月7日

## 规范 URIs

### 此版本

https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.docx (权威性)
https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html
https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.pdf

### 前一版本

http://docs.oasis-open.org/mqtt/mqtt/v5.0/cos01/mqtt-v5.0-cos01.docx (权威性)
http://docs.oasis-open.org/mqtt/mqtt/v5.0/cos01/mqtt-v5.0-cos01.html
http://docs.oasis-open.org/mqtt/mqtt/v5.0/cos01/mqtt-v5.0-cos01.pdf

### 最新版本

https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.docx (权威性)
https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html
https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.pdf

### 技术委员会

[OASIS Message Queuing Telemetry Transport (MQTT) TC](https://www.oasis-open.org/committees/mqtt/)

### 主席

Richard Coppen (coppen@uk.ibm.com), [IBM](http://www.ibm.com/)

### 编辑

Andrew Banks (andrew_banks@uk.ibm.com), [IBM](http://www.ibm.com/)
Ed Briggs (edbriggs@microsoft.com), [Microsoft](http://www.microsoft.com/)
Ken Borgendale (kwb@us.ibm.com), [IBM](http://www.ibm.com/)
Rahul Gupta (rahul.gupta@us.ibm.com), [IBM](http://www.ibm.com/)
  
### 相关工作

本规范取代：

- *MQTT 3.1.1*  由 Andrew Banks 和 Rahul Gupta 编辑发布与 2014年10月29日。 OASIS 标准 http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html 最新版本：http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html

本规范涉及：

- *MQTT and the NIST Cybersecurity Framework Version 1.0* 由 Geoff Brown 和 Louis-Philippe Lamoureux 编辑发布。最新版本： http://docs.oasis-open.org/mqtt/mqtt-nist-cybersecurity/v1.0/mqtt-nist-cybersecurity-v1.0.html

### 简介

MQTT 是一个 客户端/服务器 架构，采用 订阅/发布 模式的消息传输协议。是一套轻量级的、开放的、简单且易于实现的标准。这些特性使得他适用于多种场景，包括一些资源受限的场景比如机器和机器之间的通信（M2M）或是物联网（IoT）场景，这些场景要求较小的代码空间占用，或是网络带宽非常珍贵。

MQTT 基于 TCP/IP 或其他提供了顺序、无报文丢失、双向链接的网络协议。MQTT 的特性包括：

- 通过 订阅/发布 模式实现一对多的消息传输和应用程序解耦。
- 与负载内容无关的消息传输。
- 三种不同服务质量(QoS)的消息传输：
  - 至多一次(At most once)，根据操作环境情况尽最大努力来传输消息，消息可能会丢失。例如这种模式可以用于传感器数据采集，单次的消息的丢失并不重要，因为下一个消息很快就会到来。
  - 至少一次(At least once)，可以确保消息到达，但是可能会造成消息重复。
  - 确保一次(Exactly once)，可以确保消息只到达一次，例如这种消息可以用于账单交易信息，在交易场景下消息的丢失或者重复处理都会带来糟糕的后果。
- 小型的协议头，用来降低网络负载。
- 当发生异常断开时通知相关方的机制。

### 状态

本文档最后一次修订的日期和级别都已经在前文中描述。检查[最新版本](#最新版本)位置了解本文档后续可能的修订版。技术委员会(TC)制作的其他版本文档或其他技术项目均在此提供 https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=mqtt#technical

技术委员会成员应将对此文档的评论发送至技术委员会邮件列表，其他人需在技术委员会的网站(https://www.oasis-open.org/committees/mqtt/)订阅公共评论列表后，通过点击[发送评论](https://www.oasis-open.org/committees/comments/index.php?wg_abbrev=mqtt)将评论发送至公共评论列表。

本规范是在 OASIS [知识产权政策](https://www.oasis-open.org/policies-guidelines/ipr)的 [Non-Assertion](https://www.oasis-open.org/policies-guidelines/ipr#Non-Assertion-Mode)模式下提供的，该模式是技术委员会成立时选择的。关于是否有实施本规范依赖的已经披露的专利信息或是关于任何专利许可条款的信息，请参考技术委员会网站中的知识产权部分(https://www.oasis-open.org/committees/mqtt/ipr.php)。

请注意，本工作产品声明为规范的任何机器可读内容（[计算机语言定义](https://www.oasis-open.org/policies-guidelines/tc-process#wpComponentsCompLang)）均以单独的纯文本文件提供。 如果任何此类纯文本文件与工作产品的散文叙述性文档中的显示内容之间存在差异，则以单独的纯文本文件中的内容为准。

### 引用格式

引用本规范时，需要使用如下引用格式：

**\[mqtt-v5.0]**

MQTT Version 5.0. Edited by Andrew Banks, Ed Briggs, Ken Borgendale, and Rahul Gupta. 07 March 2019. OASIS Standard. https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html. Latest version: https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html.

# 提示

Copyright © OASIS Open 2019. All Rights Reserved.

以下文本中所有 `大写` 术语均具有 `OASIS` 知识产权政策（the `OASIS IPR Policy`）中指定的含义。 完整的[政策](https://www.oasis-open.org/policies-guidelines/ipr)可以在 `OASIS` 网站上找到。

本文档及其一本可以被复制并提供给其他人，本文档的原文或对本文档的部分引用、评论、解释说明等衍生品的制作、复制、出版和分发均没有限制，但上述许可的前提条件是上述的版权说明和本节内容必须包括在此类副本和衍生品内。并且，对于本文档本身的内容不得做任何修改，包括删除版权申明或对 `OASIS` 的引用，除非是为了 `OASIS` 技术委员会为了制作某些文件或者交付成果的需要（在这种情况下，必须遵守 OASIS 知识产权政策中规定的适用于版权的规则）或是将本文档翻译为英语之外的其他语言。

上述授予的有限权限是永久性的，`OASIS` 或其继承者或受让人不会撤销。

本文档和此处包含的信息均按`原样`提供，`OASIS 不承担任何明示或暗示的保证，包括但不限于使用此处信息不会侵犯任何所有权的任何保证或任何暗示的保证商用能力或特定用途的适用性`。

`OASIS` 要求任何 `OASIS` 方或任何其他方认为其专利主张必然会因实施本 `OASIS` 委员会规范或 `OASIS` 标准而受到侵犯时，通知 OASIS 技术委员会管理员并表明其愿意向此类人员授予专利许可。 专利权利要求的方式与制定本规范的 `OASIS` 技术委员会的 IPR 模式一致。

`OASIS` 邀请任何一方联系 `OASIS` 技术委员会管理员，如果它知道任何专利权利要求的所有权主张，如果专利持有者不愿意使用与制定本规范的 `OASIS` 技术委员会的 `IPR` 模式一致的方式。 `OASIS` 可能会在其网站上包含此类声明，但不承担任何这样做的义务。

对于可能声称与本文档中描述的技术的实施或使用有关的任何知识产权或其他权利的有效性或范围，或者此类权利下的任何许可可能或可能不可用的范围，`OASIS` 不持任何立场 ; 他也不代表他已做出任何努力来确定任何此类权利。 有关 `OASIS` 与 `OASIS` 技术委员会制定的任何文件或交付物的权利有关的程序的信息，请参见 `OASIS` 网站。 可供发布的权利主张的副本以及可供使用的许可证的任何保证，或者本 `OASIS` 委员会规范的实施者或用户尝试获得使用此类专有权利的一般许可证或许可的结果，或 `OASIS` 标准，可从 `OASIS` 技术委员会管理员处获取。 `OASIS` 不声明任何信息或知识产权列表在任何时候都是完整的，也不声明该列表中的任何权利要求实际上是基本权利要求。

名称`“OASIS”`是 `OASIS`（本规范的所有者和开发者）的商标，仅用于指代该组织及其官方输出。 `OASIS` 欢迎参考、实施和使用规范，同时保留强制执行其标记以防止误导性使用的权利。 请参阅 https://www.oasis-open.org/policies-guidelines/trademark 了解上述指南。

# 目录

- 1 [介绍](#1-介绍)
  - 1.0 [知识产权政策](#1-0-知识产权政策)
  - 1.1 [MQTT规范结构](#1-1-MQTT规范结构)
  - 1.2 [术语表](#1-2-术语表)
  - 1.3 [规范性引用](#1-3-规范性引用)
  - 1.4 [非规范性引用](#1-4-非规范性引用)
  - 1.5 [数据表示](#1-5-数据表示)
    - 1.5.1 [比特位](#1-5-1-比特位)
    - 1.5.2 [2字节整数](#1-5-2-2字节整数)
    - 1.5.3 [4字节整数](#1-5-3-4字节整数)
    - 1.5.4 [UTF-8字符串](#1-5-4-UTF-8字符串)
    - 1.5.5 [变长整数](#1-5-5-变长整数)
    - 1.5.6 [二进制数据](#1-5-6-二进制数据)
    - 1.5.7 [UTF-8字符串对](#1-5-7-UTF-8字符串对)
  - 1.6 安全性
  - 1.7 编辑约定
  - 1.8 变更历史
    - 1.8.1 MQTT v3.1.1
    - 1.8.2 MQTT v5.0
- 2 [MQTT包结构](#2-MQTT包结构)
  - 2.1 Structure of an MQTT Control Packet
    - 2.1.1 Fixed Header
    - 2.1.2 MQTT Control Packet type
    - 2.1.3 Flags
    - 2.1.4 Remaining Length
  - 2.2 Variable Header
    - 2.2.1 Packet Identifier
    - 2.2.2 Properties
      - 2.2.2.1 Property Length
      - 2.2.2.2 Property
  - 2.3 Payload
  - 2.4 Reason Code
- 3 [MQTT包](#3-MQTT包)
  - 3.1 CONNECT – Connection Request
    - 3.1.1 CONNECT Fixed Header
    - 3.1.2 CONNECT Variable Header
      - 3.1.2.1 Protocol Name
      - 3.1.2.2 Protocol Version
      - 3.1.2.3 Connect Flags
      - 3.1.2.4 Clean Start
      - 3.1.2.5 [遗嘱标识](#3-1-2-5-遗嘱标识)
      - 3.1.2.6 Will QoS
      - 3.1.2.7 Will Retain
      - 3.1.2.8 User Name Flag
      - 3.1.2.9 Password Flag
      - 3.1.2.10 Keep Alive
      - 3.1.2.11 CONNECT Properties
        - 3.1.2.11.1 Property Length
        - 3.1.2.11.2 Session Expiry Interval
        - 3.1.2.11.3 Receive Maximum
        - 3.1.2.11.4 Maximum Packet Size
        - 3.1.2.11.5 Topic Alias Maximum
        - 3.1.2.11.6 Request Response Information
        - 3.1.2.11.7 Request Problem Information
        - 3.1.2.11.8 User Property
        - 3.1.2.11.9 Authentication Method
        - 3.1.2.11.10 Authentication Data
      - 3.1.2.12 Variable Header non-normative example
    - 3.1.3 CONNECT Payload
      - 3.1.3.1 Client Identifier (ClientID)
      - 3.1.3.2 Will Properties
        - 3.1.3.2.1 Property Length
        - 3.1.3.2.2 Will Delay Interval
        - 3.1.3.2.3 Payload Format Indicator
        - 3.1.3.2.4 Message Expiry Interval
        - 3.1.3.2.5 Content Type
        - 3.1.3.2.6 Response Topic
        - 3.1.3.2.7 Correlation Data
        - 3.1.3.2.8 User Property
      - 3.1.3.3 Will Topic
      - 3.1.3.4 Will Payload
      - 3.1.3.5 User Name
      - 3.1.3.6 Password
    - 3.1.4 CONNECT Actions
  - 3.2 CONNACK – Connect acknowledgement
    - 3.2.1 CONNACK Fixed Header
    - 3.2.2 CONNACK Variable Header
      - 3.2.2.1 Connect Acknowledge Flags
        - 3.2.2.1.1 Session Present
      - 3.2.2.2 Connect Reason Code
      - 3.2.2.3 CONNACK Properties
        - 3.2.2.3.1 Property Length
        - 3.2.2.3.2 Session Expiry Interval
        - 3.2.2.3.3 Receive Maximum
        - 3.2.2.3.4 Maximum QoS
        - 3.2.2.3.5 Retain Available
        - 3.2.2.3.6 Maximum Packet Size
        - 3.2.2.3.7 Assigned Client Identifier
        - 3.2.2.3.8 Topic Alias Maximum
        - 3.2.2.3.9 Reason String
        - 3.2.2.3.10 User Property
        - 3.2.2.3.11 Wildcard Subscription Available
        - 3.2.2.3.12 Subscription Identifiers Available
        - 3.2.2.3.13 Shared Subscription Available
        - 3.2.2.3.14 Server Keep Alive
        - 3.2.2.3.15 Response Information
        - 3.2.2.3.16 Server Reference
        - 3.2.2.3.17 Authentication Method
        - 3.2.2.3.18 Authentication Data
    - 3.2.3 CONNACK Payload
  - 3.3 PUBLISH – Publish message
    - 3.3.1 PUBLISH Fixed Header
      - 3.3.1.1 DUP
      - 3.3.1.2 QoS
      - 3.3.1.3 RETAIN
      - 3.3.1.4 Remaining Length
    - 3.3.2 PUBLISH Variable Header
      - 3.3.2.1 Topic Name
      - 3.3.2.2 Packet Identifier
      - 3.3.2.3 PUBLISH Properties
        - 3.3.2.3.1 Property Length
        - 3.3.2.3.2 Payload Format Indicator
        - 3.3.2.3.3 Message Expiry Interval`
        - 3.3.2.3.4 Topic Alias
        - 3.3.2.3.5 Response Topic
        - 3.3.2.3.6 Correlation Data
        - 3.3.2.3.7 User Property
        - 3.3.2.3.8 Subscription Identifier
        - 3.3.2.3.9 Content Type
    - 3.3.3 PUBLISH Payload
    - 3.3.4 PUBLISH Actions
  - 3.4 PUBACK – Publish acknowledgement
    - 3.4.1 PUBACK Fixed Header
    - 3.4.2 PUBACK Variable Header
      - 3.4.2.1 PUBACK Reason Code
      - 3.4.2.2 PUBACK Properties
        - 3.4.2.2.1 Property Length
        - 3.4.2.2.2 Reason String
        - 3.4.2.2.3 User Property
    - 3.4.3 PUBACK Payload
    - 3.4.4 PUBACK Actions
  - 3.5 PUBREC – Publish received (QoS 2 delivery part 1)
    - 3.5.1 PUBREC Fixed Header
    - 3.5.2 PUBREC Variable Header
      - 3.5.2.1 PUBREC Reason Code
      - 3.5.2.2 PUBREC Properties
        - 3.5.2.2.1 Property Length
        - 3.5.2.2.2 Reason String
        - 3.5.2.2.3 User Property
    - 3.5.3 PUBREC Payload
    - 3.5.4 PUBREC Actions
  - 3.6 PUBREL – Publish release (QoS 2 delivery part 2)
    - 3.6.1 PUBREL Fixed Header
    - 3.6.2 PUBREL Variable Header
      - 3.6.2.1 PUBREL Reason Code
      - 3.6.2.2 PUBREL Properties
        - 3.6.2.2.1 Property Length
        - 3.6.2.2.2 Reason String
        - 3.6.2.2.3 User Property
    - 3.6.3 PUBREL Payload
    - 3.6.4 PUBREL Actions
  - 3.7 PUBCOMP – Publish complete (QoS 2 delivery part 3)
    - 3.7.1 PUBCOMP Fixed Header
    - 3.7.2 PUBCOMP Variable Header
      - 3.7.2.1 PUBCOMP Reason Code
      - 3.7.2.2 PUBCOMP Properties
        - 3.7.2.2.1 Property Length
        - 3.7.2.2.2 Reason String
        - 3.7.2.2.3 User Property
    - 3.7.3 PUBCOMP Payload
    - 3.7.4 PUBCOMP Actions
  - 3.8 SUBSCRIBE - Subscribe request
    - 3.8.1 SUBSCRIBE Fixed Header
    - 3.8.2 SUBSCRIBE Variable Header
      - 3.8.2.1 SUBSCRIBE Properties
        - 3.8.2.1.1 Property Length
        - 3.8.2.1.2 Subscription Identifier
        - 3.8.2.1.3 User Property
    - 3.8.3 SUBSCRIBE Payload
      - 3.8.3.1 Subscription Options
    - 3.8.4 SUBSCRIBE Actions
  - 3.9 SUBACK – Subscribe acknowledgement
    - 3.9.1 SUBACK Fixed Header
    - 3.9.2 SUBACK Variable Header
      - 3.9.2.1 SUBACK Properties
        - 3.9.2.1.1 Property Length
        - 3.9.2.1.2 Reason String
        - 3.9.2.1.3 User Property
    - 3.9.3 SUBACK Payload
  - 3.10 UNSUBSCRIBE – Unsubscribe request
    - 3.10.1 UNSUBSCRIBE Fixed Header
    - 3.10.2 UNSUBSCRIBE Variable Header
      - 3.10.2.1 UNSUBSCRIBE Properties
        - 3.10.2.1.1 Property Length
        - 3.10.2.1.2 User Property
    - 3.10.3 UNSUBSCRIBE Payload
    - 3.10.4 UNSUBSCRIBE Actions
  - 3.11 UNSUBACK – Unsubscribe acknowledgement
    - 3.11.1 UNSUBACK Fixed Header
    - 3.11.2 UNSUBACK Variable Header
      - 3.11.2.1 UNSUBACK Properties
        - 3.11.2.1.1 Property Length
        - 3.11.2.1.2 Reason String
        - 3.11.2.1.3 User Property
    - 3.11.3 UNSUBACK Payload
  - 3.12 PINGREQ – PING request
    - 3.12.1 PINGREQ Fixed Header
    - 3.12.2 PINGREQ Variable Header
    - 3.12.3 PINGREQ Payload
    - 3.12.4 PINGREQ Actions
  - 3.13 PINGRESP – PING response
    - 3.13.1 PINGRESP Fixed Header
    - 3.13.2 PINGRESP Variable Header
    - 3.13.3 PINGRESP Payload
    - 3.13.4 PINGRESP Actions
  - 3.14 DISCONNECT – Disconnect notification
    - 3.14.1 DISCONNECT Fixed Header
    - 3.14.2 DISCONNECT Variable Header
      - 3.14.2.1 Disconnect Reason Code
      - 3.14.2.2 DISCONNECT Properties
        - 3.14.2.2.1 Property Length
        - 3.14.2.2.2 Session Expiry Interval
        - 3.14.2.2.3 Reason String
        - 3.14.2.2.4 User Property
        - 3.14.2.2.5 Server Reference
    - 3.14.3 DISCONNECT Payload
    - 3.14.4 DISCONNECT Actions
  - 3.15 AUTH – Authentication exchange
    - 3.15.1 AUTH Fixed Header
    - 3.15.2 AUTH Variable Header
      - 3.15.2.1 Authenticate Reason Code
      - 3.15.2.2 AUTH Properties
        - 3.15.2.2.1 Property Length
        - 3.15.2.2.2 Authentication Method
        - 3.15.2.2.3 Authentication Data
        - 3.15.2.2.4 Reason String
        - 3.15.2.2.5 User Property
    - 3.15.3 AUTH Payload
    - 3.15.4 AUTH Actions
- 4 [操作行为](#4-操作行为)
  - 4.1 Session State
    - 4.1.1 Storing Session State
    - 4.1.2 Session State non-normative examples
  - 4.2 [网络连接](#4-2-网络连接)
  - 4.3 Quality of Service levels and protocol flows
    - 4.3.1 QoS 0: At most once delivery
    - 4.3.2 QoS 1: At least once delivery
    - 4.3.3 QoS 2: Exactly once delivery
  - 4.4 Message delivery retry
  - 4.5 Message receipt
  - 4.6 Message ordering
  - 4.7 [主题名和主题过滤器](#4-7-主题名和主题过滤器)
    - 4.7.1 Topic wildcards
      - 4.7.1.1 Topic level separator
      - 4.7.1.2 Multi-level wildcard
      - 4.7.1.3 Single-level wildcard
    - 4.7.2 Topics beginning with $
    - 4.7.3 Topic semantic and usage
  - 4.8 Subscriptions
    - 4.8.1 Non‑shared Subscriptions
    - 4.8.2 Shared Subscriptions
  - 4.9 Flow Control
  - 4.10 Request / Response
    - 4.10.1 Basic Request Response (non-normative)
    - 4.10.2 Determining a Response Topic value (non-normative)
  - 4.11 Server redirection
  - 4.12 Enhanced authentication
    - 4.12.1 Re-authentication
  - 4.13 [错误处理](#4-13-错误处理)
    - 4.13.1 Malformed Packet and Protocol Errors
    - 4.13.2 Other errors
- 5 [安全性 (非规范性)](#5-安全性（非规范性）)
  - 5.1 Introduction
  - 5.2 MQTT solutions: security and certification
  - 5.3 Lightweight crytography and constrained devices
  - 5.4 Implementation notes
    - 5.4.1 Authentication of Clients by the Server
    - 5.4.2 Authorization of Clients by the Server
    - 5.4.3 Authentication of the Server by the Client
    - 5.4.4 Integrity of Application Messages and MQTT Control Packets
    - 5.4.5 Privacy of Application Messages and MQTT Control Packets
    - 5.4.6 Non-repudiation of message transmission
    - 5.4.7 Detecting compromise of Clients and Servers
    - 5.4.8 Detecting abnormal behaviors
    - 5.4.9 Handling of Disallowed Unicode code points
      - 5.4.9.1 Considerations for the use of Disallowed Unicode code points
      - 5.4.9.2 Interactions between Publishers and Subscribers
      - 5.4.9.3 Remedies
    - 5.4.10 Other security considerations
    - 5.4.11 Use of SOCKS
    - 5.4.12 Security profiles
      - 5.4.12.1 Clear communication profile
      - 5.4.12.2 Secured network communication profile
      - 5.4.12.3 Secured transport profile
      - 5.4.12.4 Industry specific security profiles
- 6 [使用WebSocket作为传输层](#6-使用WebSocket作为传输层)
  - 6.1 IANA considerations
- 7 [一致性](#7-一致性)
  - 7.1 Conformance clauses
    - 7.1.1 MQTT Server conformance clause
    - 7.1.2 MQTT Client conformance clause
- Appendix A. Acknowledgments
- Appendix B. Mandatory normative statement (non-normative)
- Appendix C. Summary of new features in MQTT v5.0 (non-normative)

# 1 介绍

## 1.0 知识产权政策

本规范是在 OASIS [知识产权政策](https://www.oasis-open.org/policies-guidelines/ipr)的 [Non-Assertion](https://www.oasis-open.org/policies-guidelines/ipr#Non-Assertion-Mode)模式下提供的，该模式是技术委员会成立时选择的。关于是否有实施本规范依赖的已经披露的专利信息或是关于任何专利许可条款的信息，请参考技术委员会网站中的知识产权部分(https://www.oasis-open.org/committees/mqtt/ipr.php)。

## 1.1 MQTT规范结构

本规范分为七个章节：

- 第一章 - [介绍](#1-介绍)
- 第二章 - [MQTT包格式](#2-MQTT包结构)
- 第三章 - [MQTT包](#3-MQTT包)
- 第四章 - [操作行为](#4-操作行为)
- 第五章 - [安全性](#5-安全性（非规范性）)
- 第六章 - [使用Websocket作为传输层](#6-使用WebSocket作为传输层)
- 第七章 - [一致性目标](#7-一致性)

## 1.2 术语表

本文档中的关键字 “必须(MUST)”，“必须不(MUST NOT)”，“需要(REQUIRED)”，“应该(SHALL)”，“不应该(SHALL NOT)”，“理应(SHOULD)”，“理应不(SHOULD NOT)”，“推荐(RECOMMENDED)”，“也许(MAY)”，和“可选(OPTIONAL)”按照IETF [RFC 2119](#1.3-RFC2119)的定义阐释，除非在此类关键字出现的地方明确被标记为非规范性。

**网络连接:**

**由 MQTT 使用的传输协议提供的构造。**

- 他在客户端与服务器之间提供连接。
- 他提供了有序、无丢失、双向传输数据流的能力。

参考 [4.2 网络连接](#4-2-网络连接) 中提供的非规范性示例。

**应用消息**

提供应用程序使用的通过MQTT协议携带的消息。当应用消息通过MQTT传输时，他包含载荷，服务质量，属性集合和主题名称。

**客户端**

一个使用了 MQTT 的程序或设备。一个客户端往往：

- 打开通往服务端的网络连接
- 发布其他客户端可能会感兴趣的应用消息
- 通过订阅来接收自己感兴趣的应用消息
- 通过取消订阅，不再接收应用消息
- 关闭通过服务端的网络连接

**服务器**

一个在发布应用消息的客户端和订阅应用消息的客户端之间充当转发中介的程序或设备。一个服务器往往：

- 接收来自客户端的网络连接
- 接收客户端发布的应用消息
- 处理客户端的订阅和取消订阅请求
- 根据客户端的订阅情况匹配转发应用消息
- 关闭和客户端的网络连接

**会话**

服务器和客户端之间的有状态交互。有些会话仅和单次网络连接持续一样长的时间，有些会话则能够跨越多次连续的网络断开和连接持续保持。

**订阅**

订阅包括了主题过滤器和最大QoS。订阅只关联到一个会话。一个会话可以包括多个订阅。会话中的每个订阅都拥有一个不同的主题过滤器。

**共享订阅**

共享订阅包括了主题过滤器和最大QoS。共享订阅可以与多个会话关联，以便使用更加宽泛的信息交换模式。匹配到共享订阅的应用消息只会发送到被关联的会话其中之一对应的客户端。一个会话可以同时持有多个共享订阅，也可以同时持有共享订阅和普通订阅。

**通配订阅**

通配订阅指的是主题过滤器中包括了一个或多个通配符的订阅。通配订阅允许此订阅匹配多个主题名。参考 [4.7](#4-7-主题名和主题过滤器) 来了解通配符如何在主题过滤器中起作用。

**主题名**

附加到应用消息的文字标签，用于与服务器已知的订阅相匹配。

**主题过滤器**

订阅中包含的一种表达式，用于指示对一个或多个主题的兴趣。主题过滤器可以包含通配符。

**MQTT包**

通过网络连接发送的数据包。MQTT规范定义了十五中不同类型的MQTT包，例如 `发布` 包用来承载应用消息。

**格式错误的包**

一个不能通过本规范解析的数据包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的消息。

**协议错误**

数据包被解析后发现的不符合协议规范的数据内容或客户端与服务器状态不一致的数据。参考 [4.13](#4-13-错误处理) 查看关于错误处理的消息。

**遗嘱**

当网络连接非正常关闭后，由服务端发布的应用消息。参考 [3.1.2.5](#3-1-2-5-遗嘱标识) 查看关于遗嘱的消息。

**禁止的 Unicode 码段**
 
在 UTF-8 字符串中不应出现的 Unicode 控制代码和 Unicode 非字符集。参考 [1.5.4](#1-5-4-UTF-8字符串) 查看更多关于禁止的 Unicode 码段的信息。

## 1.3 规范性引用

<span id="1.3-RFC2119" style="font-weight:bold;">[RFC2119]</span>

Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/RFC2119, March 1997,

http://www.rfc-editor.org/info/rfc2119

<span id="1.3-RFC3629" style="font-weight:bold;">[RFC3629]</span>

Yergeau, F., "UTF-8, a transformation format of ISO 10646", STD 63, RFC 3629, DOI 10.17487/RFC3629, November 2003,

http://www.rfc-editor.org/info/rfc3629

<span id="1.3-RFC6455" style="font-weight:bold;">[RFC6455]</span>

Fette, I. and A. Melnikov, "The WebSocket Protocol", RFC 6455, DOI 10.17487/RFC6455, December 2011,

http://www.rfc-editor.org/info/rfc6455

<span id="1.3-Unicode" style="font-weight:bold;">[Unicode]</span>

The Unicode Consortium. The Unicode Standard,

http://www.unicode.org/versions/latest/

## 1.4 非规范性引用

<span id="1.4-RFC0793" style="font-weight:bold;">[RFC0793]</span>

Postel, J., "Transmission Control Protocol", STD 7, RFC 793, DOI 10.17487/RFC0793, September 1981, http://www.rfc-editor.org/info/rfc793

<span id="1.4-RFC5246" style="font-weight:bold;">[RFC5246]</span>

Dierks, T. and E. Rescorla, "The Transport Layer Security (TLS) Protocol Version 1.2", RFC 5246, DOI 10.17487/RFC5246, August 2008,

http://www.rfc-editor.org/info/rfc5246

<span id="1.4-AES" style="font-weight:bold;">[AES]</span>

Advanced Encryption Standard (AES) (FIPS PUB 197).

https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf

<span id="1.4-CHACHA20" style="font-weight:bold;">[CHACHA20]</span>

ChaCha20 and Poly1305 for IETF Protocols

https://tools.ietf.org/html/rfc7539

<span id="1.4-FIPS1402" style="font-weight:bold;">[FIPS1402]</span>

Security Requirements for Cryptographic Modules (FIPS PUB 140-2)

https://csrc.nist.gov/csrc/media/publications/fips/140/2/final/documents/fips1402.pdf

<span id="1.4-IEEE 802.1AR" style="font-weight:bold;">[IEEE 802.1AR]</span>

IEEE Standard for Local and metropolitan area networks - Secure Device Identity

http://standards.ieee.org/findstds/standard/802.1AR-2009.html

<span id="1.4-ISO29192" style="font-weight:bold;">[ISO29192]</span>

ISO/IEC 29192-1:2012 Information technology -- Security techniques -- Lightweight cryptography -- Part 1: General

https://www.iso.org/standard/56425.html

<span id="1.4-MQTT NIST" style="font-weight:bold;">[MQTT NIST]</span>

MQTT supplemental publication, MQTT and the NIST Framework for Improving Critical Infrastructure Cybersecurity

http://docs.oasis-open.org/mqtt/mqtt-nist-cybersecurity/v1.0/mqtt-nist-cybersecurity-v1.0.html

<span id="1.4-MQTTV311" style="font-weight:bold;">[MQTTV311]</span>

MQTT V3.1.1 Protocol Specification

http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html

<span id="1.4-ISO20922" style="font-weight:bold;">[ISO20922]</span>

MQTT V3.1.1 ISO Standard (ISO/IEC 20922:2016)

https://www.iso.org/standard/69466.html

<span id="1.4-NISTCSF" style="font-weight:bold;">[NISTCSF]</span>

Improving Critical Infrastructure Cybersecurity Executive Order 13636

https://www.nist.gov/sites/default/files/documents/itl/preliminary-cybersecurity-framework.pdf

<span id="1.4-NIST7628" style="font-weight:bold;">[NIST7628]</span>

NISTIR 7628 Guidelines for Smart Grid Cyber Security Catalogue

https://www.nist.gov/sites/default/files/documents/smartgrid/nistir-7628_total.pdf

<span id="1.4-NSAB" style="font-weight:bold;">[NSAB]</span>

NSA Suite B Cryptography

http://www.nsa.gov/ia/programs/suiteb_cryptography/

<span id="1.4-PCIDSS" style="font-weight:bold;">[PCIDSS]</span>

PCI-DSS Payment Card Industry Data Security Standard

https://www.pcisecuritystandards.org/pci_security/

<span id="1.4-RFC1928" style="font-weight:bold;">[RFC1928]</span>

Leech, M., Ganis, M., Lee, Y., Kuris, R., Koblas, D., and L. Jones, "SOCKS Protocol Version 5", RFC 1928, DOI 10.17487/RFC1928, March 1996,

http://www.rfc-editor.org/info/rfc1928

<span id="1.4-RFC4511" style="font-weight:bold;">[RFC4511]</span>

Sermersheim, J., Ed., "Lightweight Directory Access Protocol (LDAP): The Protocol", RFC 4511, DOI 10.17487/RFC4511, June 2006,

http://www.rfc-editor.org/info/rfc4511

<span id="1.4-RFC5280" style="font-weight:bold;">[RFC5280]</span>

Cooper, D., Santesson, S., Farrell, S., Boeyen, S., Housley, R., and W. Polk, "Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile", RFC 5280, DOI 10.17487/RFC5280, May 2008,

http://www.rfc-editor.org/info/rfc5280

<span id="1.4-RFC6066" style="font-weight:bold;">[RFC6066]</span>

Eastlake 3rd, D., "Transport Layer Security (TLS) Extensions: Extension Definitions", RFC 6066, DOI 10.17487/RFC6066, January 2011,

http://www.rfc-editor.org/info/rfc6066

<span id="1.4-RFC6749" style="font-weight:bold;">[RFC6749]</span>

Hardt, D., Ed., "The OAuth 2.0 Authorization Framework", RFC 6749, DOI 10.17487/RFC6749, October 2012,

http://www.rfc-editor.org/info/rfc6749

<span id="1.4-RFC6960" style="font-weight:bold;">[RFC6960]</span>

Santesson, S., Myers, M., Ankney, R., Malpani, A., Galperin, S., and C. Adams, "X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP", RFC 6960, DOI 10.17487/RFC6960, June 2013,

http://www.rfc-editor.org/info/rfc6960

<span id="1.4-SARBANES" style="font-weight:bold;">[SARBANES]</span>

Sarbanes-Oxley Act of 2002.

http://www.gpo.gov/fdsys/pkg/PLAW-107publ204/html/PLAW-107publ204.htm

<span id="1.4-USEUPRIVSH" style="font-weight:bold;">[USEUPRIVSH]</span>

U.S.-EU Privacy Shield Framework

https://www.privacyshield.gov

<span id="1.4-RFC3986" style="font-weight:bold;">[RFC3986]</span>

Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform Resource Identifier (URI): Generic Syntax", STD 66, RFC 3986, DOI 10.17487/RFC3986, January 2005,

http://www.rfc-editor.org/info/rfc3986

<span id="1.4-RFC1035" style="font-weight:bold;">[RFC1035]</span>

Mockapetris, P., "Domain names - implementation and specification", STD 13, RFC 1035, DOI 10.17487/RFC1035, November 1987,

http://www.rfc-editor.org/info/rfc1035

<span id="1.4-RFC2782" style="font-weight:bold;">[RFC2782]</span>

Gulbrandsen, A., Vixie, P., and L. Esibov, "A DNS RR for specifying the location of services (DNS SRV)", RFC 2782, DOI 10.17487/RFC2782, February 2000,

http://www.rfc-editor.org/info/rfc2782

## 1.5 数据表示

### 1.5.1 比特位

字节中的比特位被标记为7-0，最高位为7，最低位为0。

### 1.5.2 2字节整数

2字节整数指16位的大端表示的无符号整数：高位字节在低位字节之前。这意味着在网络中传输的2字节整数先传输高有效字节(MSB)，后传输低有效字节(LSB)。

### 1.5.3 4字节整数

4字节整数指32位的大端表示的无符号整数：高位字节先于连续的低位字节。这意味着在网络中传输的4字节整数先传输最高有效字节(MSB)，再传输次高有效字节(MSB)，再传输次高有效字节(MSB)，最后传输低有效字节(LSB)。

### 1.5.4 UTF-8字符串

### 1.5.5 变长整数

### 1.5.6 二进制数据

### 1.5.7 UTF-8字符串对

# 2 MQTT包结构

# 3 MQTT包

#### 3.1.2.5 遗嘱标识

# 4 操作行为

## 4.2 网络连接

## 4.7 主题名和主题过滤器

## 4.13 错误处理

# 5 安全性（非规范性）

# 6 使用WebSocket作为传输层

# 7 一致性