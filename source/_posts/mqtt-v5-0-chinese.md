---
title: MQTT 5.0 中文文档(施工中)
date: 2024-01-06 10:14:39
categories:
- MQTT
tags:
- MQTT
- 网络协议
---

<head>
  <style>
    :root {
      --vc-marked: #ffc107;
      --vc-referred: #EE0000;
    }
    [data-user-color-scheme="dark"] {
      --vc-marked: #886c57;
      --vc-referred: #EE0000;
    }
    .bold {
      font-weight: bold;
    }
    .vcLinked {
      color: var(--post-link-color);
    }
    .vcMarked {
      background: var(--vc-marked);
    }
    .vcReferred {
      color: var(--vc-referred);
    }
  </style>
</head>

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
  - 1.6 [安全性](#1-6-安全性)
  - 1.7 [编辑约定](#1-7-编辑约定)
  - 1.8 [变更历史](#1-8-变更历史)
    - 1.8.1 [MQTT v3.1.1](#1-8-1-MQTT-v3-1-1)
    - 1.8.2 [MQTT v5.0](#1-8-2-MQTT-v5-0)
- 2 [MQTT包格式](#2-MQTT包格式)
  - 2.1 [MQTT包结构](#2-1-MQTT包结构)
    - 2.1.1 [固定头](#2-1-1-固定头)
    - 2.1.2 [MQTT包类型](#2-1-2-MQTT包类型)
    - 2.1.3 [控制标识](2-1-3-控制标识)
    - 2.1.4 [剩余长度](#2-1-4-剩余长度)
  - 2.2 [可变头](#2-2-可变头)
    - 2.2.1 [包ID](#2-2-1-包ID)
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
    - 3.3.1 [PUBLISH 固定头](#3-3-1-PUBLISH-固定头)
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
- 5 [安全性（非规范性）](#5-安全性（非规范性）)
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
    - 5.4.9 [处理禁止的Unicode码段](#5-4-9-处理禁止的Unicode码段)
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
- 附录 C. [MQTT v5.0 新特性汇总（非规范性）](#附录-C-MQTT-v5-0-新特性汇总（非规范性）)

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

一个不能通过本规范解析的数据包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

**协议错误**

数据包被解析后发现的不符合协议规范的数据内容或客户端与服务器状态不一致的数据。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

**遗嘱**

当网络连接非正常关闭后，由服务端发布的应用消息。参考 [3.1.2.5](#3-1-2-5-遗嘱标识) 查看关于遗嘱的消息。

**禁止的 Unicode 码段**
 
在 UTF-8 字符串中不应出现的 Unicode 控制代码和 Unicode 非字符集。参考 [1.5.4](#1-5-4-UTF-8字符串) 查看更多关于禁止的 Unicode 码段的信息。

## 1.3 规范性引用

<span id="1.3-RFC2119" class="bold">[RFC2119]</span>

Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/RFC2119, March 1997,

http://www.rfc-editor.org/info/rfc2119

<span id="1.3-RFC3629" class="bold">[RFC3629]</span>

Yergeau, F., "UTF-8, a transformation format of ISO 10646", STD 63, RFC 3629, DOI 10.17487/RFC3629, November 2003,

http://www.rfc-editor.org/info/rfc3629

<span id="1.3-RFC6455" class="bold">[RFC6455]</span>

Fette, I. and A. Melnikov, "The WebSocket Protocol", RFC 6455, DOI 10.17487/RFC6455, December 2011,

http://www.rfc-editor.org/info/rfc6455

<span id="1.3-Unicode" class="bold">[Unicode]</span>

The Unicode Consortium. The Unicode Standard,

http://www.unicode.org/versions/latest/

## 1.4 非规范性引用

<span id="1.4-RFC0793" class="bold">[RFC0793]</span>

Postel, J., "Transmission Control Protocol", STD 7, RFC 793, DOI 10.17487/RFC0793, September 1981, http://www.rfc-editor.org/info/rfc793

<span id="1.4-RFC5246" class="bold">[RFC5246]</span>

Dierks, T. and E. Rescorla, "The Transport Layer Security (TLS) Protocol Version 1.2", RFC 5246, DOI 10.17487/RFC5246, August 2008,

http://www.rfc-editor.org/info/rfc5246

<span id="1.4-AES" class="bold">[AES]</span>

Advanced Encryption Standard (AES) (FIPS PUB 197).

https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf

<span id="1.4-CHACHA20" class="bold">[CHACHA20]</span>

ChaCha20 and Poly1305 for IETF Protocols

https://tools.ietf.org/html/rfc7539

<span id="1.4-FIPS1402" class="bold">[FIPS1402]</span>

Security Requirements for Cryptographic Modules (FIPS PUB 140-2)

https://csrc.nist.gov/csrc/media/publications/fips/140/2/final/documents/fips1402.pdf

<span id="1.4-IEEE 802.1AR" class="bold">[IEEE 802.1AR]</span>

IEEE Standard for Local and metropolitan area networks - Secure Device Identity

http://standards.ieee.org/findstds/standard/802.1AR-2009.html

<span id="1.4-ISO29192" class="bold">[ISO29192]</span>

ISO/IEC 29192-1:2012 Information technology -- Security techniques -- Lightweight cryptography -- Part 1: General

https://www.iso.org/standard/56425.html

<span id="1.4-MQTT NIST" class="bold">[MQTT NIST]</span>

MQTT supplemental publication, MQTT and the NIST Framework for Improving Critical Infrastructure Cybersecurity

http://docs.oasis-open.org/mqtt/mqtt-nist-cybersecurity/v1.0/mqtt-nist-cybersecurity-v1.0.html

<span id="1.4-MQTTV311" class="bold">[MQTTV311]</span>

MQTT V3.1.1 Protocol Specification

http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html

<span id="1.4-ISO20922" class="bold">[ISO20922]</span>

MQTT V3.1.1 ISO Standard (ISO/IEC 20922:2016)

https://www.iso.org/standard/69466.html

<span id="1.4-NISTCSF" class="bold">[NISTCSF]</span>

Improving Critical Infrastructure Cybersecurity Executive Order 13636

https://www.nist.gov/sites/default/files/documents/itl/preliminary-cybersecurity-framework.pdf

<span id="1.4-NIST7628" class="bold">[NIST7628]</span>

NISTIR 7628 Guidelines for Smart Grid Cyber Security Catalogue

https://www.nist.gov/sites/default/files/documents/smartgrid/nistir-7628_total.pdf

<span id="1.4-NSAB" class="bold">[NSAB]</span>

NSA Suite B Cryptography

http://www.nsa.gov/ia/programs/suiteb_cryptography/

<span id="1.4-PCIDSS" class="bold">[PCIDSS]</span>

PCI-DSS Payment Card Industry Data Security Standard

https://www.pcisecuritystandards.org/pci_security/

<span id="1.4-RFC1928" class="bold">[RFC1928]</span>

Leech, M., Ganis, M., Lee, Y., Kuris, R., Koblas, D., and L. Jones, "SOCKS Protocol Version 5", RFC 1928, DOI 10.17487/RFC1928, March 1996,

http://www.rfc-editor.org/info/rfc1928

<span id="1.4-RFC4511" class="bold">[RFC4511]</span>

Sermersheim, J., Ed., "Lightweight Directory Access Protocol (LDAP): The Protocol", RFC 4511, DOI 10.17487/RFC4511, June 2006,

http://www.rfc-editor.org/info/rfc4511

<span id="1.4-RFC5280" class="bold">[RFC5280]</span>

Cooper, D., Santesson, S., Farrell, S., Boeyen, S., Housley, R., and W. Polk, "Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile", RFC 5280, DOI 10.17487/RFC5280, May 2008,

http://www.rfc-editor.org/info/rfc5280

<span id="1.4-RFC6066" class="bold">[RFC6066]</span>

Eastlake 3rd, D., "Transport Layer Security (TLS) Extensions: Extension Definitions", RFC 6066, DOI 10.17487/RFC6066, January 2011,

http://www.rfc-editor.org/info/rfc6066

<span id="1.4-RFC6749" class="bold">[RFC6749]</span>

Hardt, D., Ed., "The OAuth 2.0 Authorization Framework", RFC 6749, DOI 10.17487/RFC6749, October 2012,

http://www.rfc-editor.org/info/rfc6749

<span id="1.4-RFC6960" class="bold">[RFC6960]</span>

Santesson, S., Myers, M., Ankney, R., Malpani, A., Galperin, S., and C. Adams, "X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP", RFC 6960, DOI 10.17487/RFC6960, June 2013,

http://www.rfc-editor.org/info/rfc6960

<span id="1.4-SARBANES" class="bold">[SARBANES]</span>

Sarbanes-Oxley Act of 2002.

http://www.gpo.gov/fdsys/pkg/PLAW-107publ204/html/PLAW-107publ204.htm

<span id="1.4-USEUPRIVSH" class="bold">[USEUPRIVSH]</span>

U.S.-EU Privacy Shield Framework

https://www.privacyshield.gov

<span id="1.4-RFC3986" class="bold">[RFC3986]</span>

Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform Resource Identifier (URI): Generic Syntax", STD 66, RFC 3986, DOI 10.17487/RFC3986, January 2005,

http://www.rfc-editor.org/info/rfc3986

<span id="1.4-RFC1035" class="bold">[RFC1035]</span>

Mockapetris, P., "Domain names - implementation and specification", STD 13, RFC 1035, DOI 10.17487/RFC1035, November 1987,

http://www.rfc-editor.org/info/rfc1035

<span id="1.4-RFC2782" class="bold">[RFC2782]</span>

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

MQTT包中使用的文本类型字段均采用 UTF-8 编码。UTF-8 [RFC3629](#1.3-RFC3629) 是一种高效的 [Unicode](#1.3-Unicode) 编码，他优化了 ACSII 的编码以用来支持基于文本的通信。

每个 UTF-8 编码的字符串都使用开头的两个字节表示字符串的长度，就像下方的 <span class="vcLinked">图1-1 UTF-8 字符串结构</span> 示意的那样。因此，UTF-8 字符串的最大长度为65535字节。

除非另有说明，否则所有 UTF-8 编码字符串可以具有 0 到 65,535 字节范围内的任意长度。

图1-1 UTF-8 字符串结构
<table>
    <tr>
        <th>Bit</th>
        <th>7</th>
        <th>6</th>
        <th>5</th>
        <th>4</th>
        <th>3</th>
        <th>2</th>
        <th>1</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 1</td>
        <td colspan="8">字符串长度高字节(MSB)</td>
    </tr>
    <tr>
        <td>byte 2</td>
        <td colspan="8">字符串长度低字节(LSB)</td>
    </tr>
    <tr>
        <td>byte 3 ...</td>
        <td colspan="8">当长度 > 0 时, UTF-8 编码的字符数据</td>
    </tr>
</table>

<span class="vcMarked">在 UTF-8 编码字符串中的字符必须为 <a href="#1.3-Unicode">[Unicode]</a> 和 <a href="#1.3-RFC3629">[RFC3629]</a> 中所定义的，格式正确的字符编码。尤其不能使用U+D800 至 U+DFFF之间的编码</span> <span class="vcReferred">[MQTT-1.5.4-1]</span>。如果客户端或服务器接收到的 MQTT 包中包括了非法 UTF-8 编码，将其视为一个格式错误的包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

<span class="vcMarked">UTF-8 编码字符串必须不包含空字符 U+0000</span> <span class="vcReferred">[MQTT-1.5.4-2]</span>。如果一个接收者（服务器或客户端）接收的 MQTT 包其中有空字符 U+0000 ，将其视为一个格式错误的包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

数据不应该包括下方列表中的 Unicode 码段，如果一个接收者（服务器或客户端）接受的 MQTT 包其中有此类字符，接收者可以将此包视为一个格式错误的包。这些是禁止使用的 Unicode 码段。参考 [5.4.9](#5-4-9-处理禁止的Unicode码段) 查看关于错误处理的信息。

- U+0001..U+001F 控制字符
- U+007F..U+009F 控制字符
- [Unicode](#1.3-Unicode) 规范中定义的非文本字符（例如 U+0FFFF）

<span class="vcMarked">无论 UTF-8 编码序列 0xEF 0xBB 0xBF 出现在字符串的何处，他永远被解释为 U+FEFF (0宽无换行空格) 而且不能被数据包的接收者跳过或剥离</span> <span class="vcReferred">[MQTT-1.5.4-3]</span>。

**非规范性示例**

例如，字符串 A𪛔 的第一个字符是拉丁文大写字母A，第二个字符是码点 U+2A6D4 （代表 CJK IDEOGRAPH EXTENSION B 字符），他是这样编码的：

图1‑2 UTF-8 编码字符串非规范性示例
<table>
    <tr>
        <th>Bit</th>
        <th>7</th>
        <th>6</th>
        <th>5</th>
        <th>4</th>
        <th>3</th>
        <th>2</th>
        <th>1</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 1</td>
        <td colspan="8">字符串长度高字节(MSB)(0x00)</td>
    </tr>
    <tr>
        <td></td>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 2</td>
        <td colspan="8">字符串长度低字节(LSB)(0x05)</td>
    </tr>
    <tr>
        <td></td>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 3</td>
        <td colspan="8">‘A’ (0x41)</td>
    </tr>
    <tr>
        <td></td>
        <th>0</th>
        <th>1</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>1</th>
    </tr>
    <tr>
        <td>byte 4</td>
        <td colspan="8">(0xF0)</td>
    </tr>
    <tr>
        <td></td>
        <th>1</th>
        <th>1</th>
        <th>1</th>
        <th>1</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 5</td>
        <td colspan="8">(0xAA)</td>
    </tr>
    <tr>
        <td></td>
        <th>1</th>
        <th>0</th>
        <th>1</th>
        <th>0</th>
        <th>1</th>
        <th>0</th>
        <th>1</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 6</td>
        <td colspan="8">(0x9B)</td>
    </tr>
    <tr>
        <td></td>
        <th>1</th>
        <th>0</th>
        <th>0</th>
        <th>1</th>
        <th>1</th>
        <th>0</th>
        <th>1</th>
        <th>1</th>
    </tr>
    <tr>
        <td>byte 7</td>
        <td colspan="8">(0x94)</td>
    </tr>
    <tr>
        <td></td>
        <th>1</th>
        <th>0</th>
        <th>0</th>
        <th>1</th>
        <th>0</th>
        <th>1</th>
        <th>0</th>
        <th>0</th>
    </tr>
</table>

### 1.5.5 变长整数

变长整数中将一个字节的最大值视为 127，更大的值采用如下方式处理。每个字节中较低的七位用来存储数字的值，最高位用来表示是否有后续的字节。因此，每个字节都编码了128个可能的值和一个 “后续位”。变长整数的最大字节数是4。<span class="vcMarked">变长整数编码时必须使用能够表示数字值的最小长度来进行编码</span> <span class="vcReferred">[MQTT-1.5.5-1]</span>。表1-1 中展示了变长整数可以表示的值。

表1-1 变长整数的值
| 位数 | 最小值 | 最大值 |
| --- | --- | --- |
| 1 | 0 (0x00) | 127 (0x7F) |
| 2 | 128 (0x80, 0x01) | 16,383 (0xFF, 0x7F) |
| 3 | 16,384 (0x80, 0x80, 0x01) | 2,097,151 (0xFF, 0xFF, 0x7F) |
| 4 | 2,097,152 (0x80, 0x80, 0x80, 0x01) | 268,435,455 (0xFF, 0xFF, 0xFF, 0x7F) |

**非规范性示例**

将非负整数 X 编码为变长整数的伪代码：

```text
do
   encodedByte = X MOD 128
   X = X DIV 128
   // if there are more data to encode, set the top bit of this byte
   if (X > 0)
      encodedByte = encodedByte OR 128
   endif
   'output' encodedByte
while (X > 0)
```

上例中的 MOD 表示取模（C语言中的 %），DIV表示整数除法（C语言中的 /），OR表示位运算中的或（C语言中的 |）。

**非规范性示例**

解码变长整数的伪代码：

```text
multiplier = 1
value = 0
do
   encodedByte = 'next byte from stream'
   value += (encodedByte AND 127) * multiplier
   if (multiplier > 128*128*128)
      throw Error(Malformed Variable Byte Integer)
   multiplier *= 128
while ((encodedByte AND 128) != 0)
```

上例中的 AND 表示位运算中的且（C语言中的 &）。

当该算法完成时，value的值即是变长整数表示的值。

### 1.5.6 二进制数据

二进制数据由2字节整数表示的字节流长度加上实际的字节流内容组成。因此，二进制数据的长度范围是 0-65535 字节。

### 1.5.7 UTF-8字符串对

UT-8 字符串对包括两个 UTF-8 编码的字符串。这种数据类型用来存储 键-值 对。第一个字符串表示键，第二个字符串表示值。

<span class="vcMarked">UTF-8字符串对中的两个字符串都必须遵守 UTF-8 字符串的需求</span> <span class="vcReferred">[MQTT-1.5.7-1]</span>。如果一个接收者（客户端或服务器）接收到的键值对没有遵守这些需求，则被视为一个格式错误的数据包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

## 1.6 安全性

MQTT 的客户端和服务器实现应该提供认证、授权和加密传输选项，这一部分在第五章讨论。强烈建议与关键基础设施、个人身份信息或其他个人或敏感信息相关的应用程序使用这些安全功能。

## 1.7 编辑约定

本规范中以<span class="vcMarked">黄色</span>突出显示的文本标识了一致性声明。每个一致性声明都被分配了一个格式为 <span class="vcReferred">[MQTT-x.x.x-y]</span> 的引用，其中 <span class="vcReferred">x.x.x</span> 是章节序号，<span class="vcReferred">y</span> 是章节内的序号。

## 1.8 变更历史

### 1.8.1 MQTT v3.1.1

MQTT v3.1.1 是 OASIS 提出的第一个 MQTT 标准 **\[MQTTV311]** 。

MQTT v3.1.1 同时也是 ISO/IEC 20922:2016 标准 [ISO20922](#1.4-ISO20922)。

### 1.8.2 MQTT v5.0

MQTT v5.0 为 MQTT 添加了大量新功能，同时保留了大部分核心功能。主要功能目标是：

- 可扩展性和大型系统的增强
- 改进错误报告能力
- 将常见用法规范化，包括功能发现和请求响应
- 包括用户属性在内的拓展机制
- 性能改进，增强对小型客户端的支持

参考 [附录 C](#附录-C-MQTT-v5-0-新特性汇总（非规范性）) 查阅 MQTT v5.0 的变更汇总。

# 2 MQTT包格式

## 2.1 MQTT包结构

MQTT 协议操作通过一系列的 MQTT 包交互来实现。本章用来描述这些 MQTT 包的结构。

一个 MQTT 包由三部分构成，顺序固定，参考下图：

<table>
  <tr><td>固定头，所有的 MQTT 包都必须持有</td><tr>
  <tr><td>可变头，部分 MQTT 包持有</td></tr>
  <tr><td>载荷，部分 MQTT 包持有</td></tr>
</table>

### 2.1.1 固定头

每个 MQTT 包都包含着一个下图所示的固定头：

图2-2 固定头格式

<table>
    <tr>
        <th>Bit</th>
        <th>7</th>
        <th>6</th>
        <th>5</th>
        <th>4</th>
        <th>3</th>
        <th>2</th>
        <th>1</th>
        <th>0</th>
    </tr>
    <tr>
        <td>byte 1</td>
        <td colspan="4">MQTT包类型</td>
        <td colspan="4">针对不同包类型的控制标识</td>
    </tr>
    <tr>
        <td>byte 2...</td>
        <td colspan="8">剩余长度</td>
    </tr>
</table>

### 2.1.2 MQTT包类型

**位置：**第一个Byte，比特位7-4。

表示为 4bit 的无符号整数，数值的含义如下表所示：

表2-1 MQTT包类型

| Name | Value | Direction of flow | Description |
| --- | --- | --- | --- |
| 保留 | 0 | 禁止 | 保留 |
| CONNECT | 1 | 客户端到服务器 | 连接请求 |
| CONNACK | 2 | 服务器到客户端 | 连接回复 |
| PUBLISH | 3 | 双向 | 消息发布 |
| PUBACK | 4 | 双向 | 消息回复（QoS1） |
| PUBREC | 5 | 双向 | 消息已接收（QoS2交付第 1 部分） |
| PUBREL | 6 | 双向 | 消息释放（QoS2交付第 2 部分） |
| PUBCOMP | 7 | 双向 | 消息完成（QoS2交付第 3 部分） |
| SUBSCRIBE | 8 | 客户端到服务器 | 订阅请求 |
| SUBACK | 9 | 服务器到客户端 | 订阅回复 |
| UNSUBSCRIBE | 10 | 客户端到服务器 | 取消订阅请求 |
| UNSUBACK | 11 | 服务器到客户端 | 取消订阅回复 |
| PINGREQ | 12 | 客户端到服务器 | PING 请求 |
| PINGRESP | 13 | 服务器到客户端 | PING 响应 |
| DISCONNECT | 14 | 双向 | 断开连接通知 |
| AUTH | 15 | 双向 | 认证交换 |

### 2.1.3 控制标识

固定头第一个 byte 中剩下的四个bit \[3-0]包括了基于不同 MQTT 包类型的控制标识。<span class="vcMarked">当一个比特位被标记为 “保留” 时，他的意义被保留到未来使用而他的值必须按照下表设置</span> <span class="vcReferred">[MQTT-2.1.3-1]</span>。如果接收到的控制标志不符合规范，则被认为是一个格式错误的数据包。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

表2‑2 控制标志

<table>
  <thead>
    <td>MQTT包</td>
    <td>控制标志</td>
    <td>Bit 3</td>
    <td>Bit 2</td>
    <td>Bit 1</td>
    <td>Bit 0</td>
  </thead>
  <tr>
    <td>CONNECT</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>CONNACK</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PUBLISH</td>
    <td>MQTT 5.0版本使用</td>
    <td>DUP</td>
    <td colspan="2">QoS</td>
    <td>RETAIN</td>
  </tr>
  <tr>
    <td>PUBACK</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PUBREC</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PUBREL</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PUBCOMP</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>SUBSCRIBE</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>SUBACK</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>UNSUBSCRIBE</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>UNSUBACK</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PINGREQ</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>PINGRESP</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>DISCONNECT</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>AUTH</td>
    <td>Reserved</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
</table>

DUP = 重复发送的 PUBLISH 包
QoS = PUBLISH 包的服务质量标识
RETAIN = PUBLISH 保留信息标识

参考 [3.3.1](#3-3-1-PUBLISH-固定头) 了解更多关于 DUP，QoS 和 RETAIN 标识在 PUBLISH 中的使用方式。

### 2.1.4 剩余长度

**位置：**从第二个Byte开始。

剩余长度是一个变长整数，用来表示当前包剩余的字节数，包括可变头和载荷。剩余长度的值不包括剩余长度自己本身占用的字节数。一个 MQTT 包完整的字节数等于固定头的长度加上剩余长度的值。

## 2.2 可变头

某些类型的 MQTT 包包含可变头。他位于固定头和载荷之间。可变头的内容根据数据包的类型变化。可变头中的包ID字段多种类型的数据包中都存在。

### 2.2.1 包ID

很多类型的 MQTT 包都在其可变头中包含了 2byte 的包ID字段。这些 MQTT 包是 PUBLISH （当 QoS > 0时），PUBACK，PUBREC，PUBREL，PUBCOMP，SUBSCRIBE，SUBACK，UNSUBSCRIBE，UNSUBACK。

需要包ID的 MQTT 包类型如下表所示：

表2-3 包含包ID的 MQTT 包类型

| MQTT 包 | 包ID字段 |
| --- | --- |
| CONNECT | 否 |
| CONNACK | 否 |
| PUBLISH | 是（仅当 QoS > 0 时） |
| PUBACK | 是 |
| PUBREC | 是 |
| PUBREL | 是 |
| PUBCOMP | 是 |
| SUBSCRIBE | 是 |
| SUBACK | 是 |
| UNSUBSCRIBE | 是 |
| UNSUBACK | 是 |
| PINGREQ | 否 |
| PINGRESP | 否 |
| DISCONNECT | 否 |
| AUTH | 否 |

<span class="vcMarked">当 PUBLISH 包的 QoS 值为 0 时，必须不包含 包ID 字段</span> <span class="vcReferred">[MQTT-2.2.1-2]</span>。

<span class="vcMarked">每当客户端发送新的 SUBSCRIBE包，UNSUBSCRIBE包 或 QoS > 0 的 PUBLISH包，必须携带一个非零且当前未被使用的 包ID</span> <span class="vcReferred">[MQTT-2.2.1-3]</span>。

<span class="vcMarked">每当服务端发送新的 QoS > 0 的 PUBLISH包，必须携带一个非零且当前未被使用的 包ID</span> <span class="vcReferred">[MQTT-2.2.1-4]</span>。

包ID仅在发送者处理了对应的回复后可重新使用，定义如下。对于 QoS = 1 的 PUBLISH，对应的回复是 PUBACK；对于 QoS = 2 的PUBLISH，对应的回复是 PUBCOMP 或当原因码为 128 或更大时为 PUBREC。对于 SUBSCRIBE 或 UNSUBSCRIBE，对应的回复是 SUBACK 或 UNSUBACK。

在一个会话中，客户端与服务器分别使用一个单独、统一的集合用作提供 PUBLISH、SUBSCRIBE 和 UNSUBSCRIBE 的包ID。包ID在任何时候都不能被多个命令使用。

<span class="vcMarked">PUBACK，PUBREC，PUBREL 或 PUBCOMP 包必须携带和 PUBLISH 相同的 包ID</span> <span class="vcReferred">[MQTT-2.2.1-5]</span>。<span class="vcMarked">SUBACK 和 UNSUBACK 必须携带和其对应的 SUBSCRIBE 和 UNSUBSCRIBE 包相同的包ID</span> <span class="vcReferred">[MQTT-2.2.1-6]</span>。

客户端与服务器各自独立的维护包ID分配。因此，客户端与服务器可以同时使用同样的包ID发送信息。

**非规范性示例**

客户端发送一个包ID为 0x1234 的 PUBLISH 包，之后在其接收到对应的 PUBACK 之前，从服务器接收到一个 包ID 为 0x1234 的 PUBLISH 包。这样的情况是合理而且完全有可能的。

```text
Client                                                   Server
PUBLISH 包ID=0x1234 ‒→
                                                    ←‒ PUBLISH 包ID=0x1234
PUBACK 包ID=0x1234 ‒→
                                                    ←‒ PUBACK 包ID=0x1234
```

# 3 MQTT包

#### 3.1.2.5 遗嘱标识

### 3.3.1 PUBLISH 固定头

# 4 操作行为

## 4.2 网络连接

## 4.7 主题名和主题过滤器

## 4.13 错误处理

# 5 安全性（非规范性）

### 5.4.9 处理禁止的Unicode码段

# 6 使用WebSocket作为传输层

# 7 一致性

# 附录 C. MQTT v5.0 新特性汇总（非规范性）