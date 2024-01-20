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

MQTT 基于 TCP/IP 或其他提供了顺序、无包丢失、双向链接的网络协议。MQTT 的特性包括：

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
    - 2.2.2 [属性集](#2-2-2-属性集)
      - 2.2.2.1 [属性长度](#2-2-2-1-属性长度)
      - 2.2.2.2 [属性](#2-2-2-2-属性)
  - 2.3 [载荷](#2-3-载荷)
  - 2.4 [原因码](#2-4-原因码)
- 3 [MQTT包](#3-MQTT包)
  - 3.1 [CONNECT - 连接请求](#3-1-CONNECT-连接请求)
    - 3.1.1 [CONNECT固定头](#3-1-1-CONNECT固定头)
    - 3.1.2 [CONNECT可变头](#3-1-2-CONNECT可变头)
      - 3.1.2.1 [协议名](#3-1-2-1-协议名)
      - 3.1.2.2 [协议版本](#3-1-2-2-协议版本)
      - 3.1.2.3 [连接标识](#3-1-2-3-连接标识)
      - 3.1.2.4 [全新开始](#3-1-2-4-全新开始)
      - 3.1.2.5 [遗嘱标识](#3-1-2-5-遗嘱标识)
      - 3.1.2.6 [遗嘱QoS](#3-1-2-6-遗嘱QoS)
      - 3.1.2.7 [遗嘱保留消息](#3-1-2-7-遗嘱保留消息)
      - 3.1.2.8 [用户名标识](#3-1-2-8-用户名标识)
      - 3.1.2.9 [密码标识](#3-1-2-9-密码标识)
      - 3.1.2.10 [保活时间](#3-1-2-10-保活时间)
      - 3.1.2.11 [CONNECT 属性集](#3-1-2-11-CONNECT-属性集)
        - 3.1.2.11.1 [属性长度](#3-1-2-11-1-属性长度)
        - 3.1.2.11.2 [会话过期间隔](#3-1-2-11-2-会话过期间隔)
        - 3.1.2.11.3 [接收最大值](#3-1-2-11-3-接收最大值)
        - 3.1.2.11.4 [最大包尺寸](#3-1-2-11-4-最大包尺寸)
        - 3.1.2.11.5 [主题别名最大值](#3-1-2-11-5-主题别名最大值)
        - 3.1.2.11.6 [请求响应信息](#3-1-2-11-6-请求响应信息)
        - 3.1.2.11.7 [请求问题信息](#3-1-2-11-7-请求问题信息)
        - 3.1.2.11.8 [用户属性](#3-1-2-11-8-用户属性)
        - 3.1.2.11.9 [认证方式](#3-1-2-11-9-认证方式)
        - 3.1.2.11.10 [认证数据](#3-1-2-11-10-认证数据)
      - 3.1.2.12 [可变头非规范性示例](#3-1-2-12-可变头非规范性示例)
    - 3.1.3 [CONNECT载荷](#3-1-3-CONNECT载荷)
      - 3.1.3.1 [客户端ID](#3-1-3-1-客户端ID)
      - 3.1.3.2 [遗嘱属性集](#3-1-3-2-遗嘱属性集)
        - 3.1.3.2.1 [属性长度](#3-1-3-2-1-属性长度)
        - 3.1.3.2.2 [遗嘱延迟间隔](#3-1-3-2-2-遗嘱延迟间隔)
        - 3.1.3.2.3 [载荷格式标识](#3-1-3-2-3-载荷格式标识)
        - 3.1.3.2.4 [消息过期间隔](#3-1-3-2-4-消息过期间隔)
        - 3.1.3.2.5 [内容类型](#3-1-3-2-5-内容类型)
        - 3.1.3.2.6 [响应主题](#3-1-3-2-6-响应主题)
        - 3.1.3.2.7 [关联数据](#3-1-3-2-7-关联数据)
        - 3.1.3.2.8 [用户属性](#3-1-3-2-8-用户属性)
      - 3.1.3.3 [遗嘱主题](#3-1-3-3-遗嘱主题)
      - 3.1.3.4 [遗嘱载荷](3-1-3-4-遗嘱载荷)
      - 3.1.3.5 [用户名](#3-1-3-5-用户名)
      - 3.1.3.6 [密码](#3-1-3-6-密码)
    - 3.1.4 [CONNECT动作](#3-1-4-CONNECT动作)
  - 3.2 [CONNACK – 连接回复](#3-2-CONNACK-–-连接回复)
    - 3.2.1 [CONNACK固定头](#3-2-1-CONNACK固定头)
    - 3.2.2 [CONNACK可变头](#3-2-2-CONNACK可变头)
      - 3.2.2.1 [连接回复标识](#3-2-2-1-连接回复标识)
        - 3.2.2.1.1 [会话展示](#3-2-2-1-1-会话展示)
      - 3.2.2.2 [连接原因码](#3-2-2-2-连接原因码)
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
  - 4.1 [会话状态](#4-1-会话状态)
    - 4.1.1 [存储会话状态](#4-1-1-存储会话状态)
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
  - 4.9 [流量控制](#4-9-流量控制)
  - 4.10 [请求 / 响应](#4-10-请求-响应)
    - 4.10.1 Basic Request Response (non-normative)
    - 4.10.2 Determining a Response Topic value (non-normative)
  - 4.11 Server redirection
  - 4.12 [增强认证](#4-12-增强认证)
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
  <thead>
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
  </thead>
  <tbody>
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
  </tbody>
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
  <thead>
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
  </thead>
  <tbody>
    <tr>
      <td>byte 1</td>
      <td colspan="8">字符串长度高字节(MSB)(0x00)</td>
    </tr>
    <tr>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>byte 2</td>
      <td colspan="8">字符串长度低字节(LSB)(0x05)</td>
    </tr>
    <tr>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>byte 3</td>
      <td colspan="8">‘A’ (0x41)</td>
    </tr>
    <tr>
      <td></td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>byte 4</td>
      <td colspan="8">(0xF0)</td>
    </tr>
    <tr>
      <td></td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>byte 5</td>
      <td colspan="8">(0xAA)</td>
    </tr>
    <tr>
      <td></td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <td>byte 6</td>
      <td colspan="8">(0x9B)</td>
    </tr>
    <tr>
      <td></td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>byte 7</td>
      <td colspan="8">(0x94)</td>
    </tr>
    <tr>
      <td></td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
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
  <tbody>
    <tr><td>固定头，所有的 MQTT 包都必须持有</td><tr>
    <tr><td>可变头，部分 MQTT 包持有</td></tr>
    <tr><td>载荷，部分 MQTT 包持有</td></tr>
  </tbody>
</table>

### 2.1.1 固定头

每个 MQTT 包都包含着一个下图所示的固定头：

图2-2 固定头格式

<table>
  <thead>
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
  </thead>
  <tbody>
    <tr>
      <td>byte 1</td>
      <td colspan="4">MQTT包类型</td>
      <td colspan="4">针对不同包类型的控制标识</td>
    </tr>
    <tr>
      <td>byte 2...</td>
      <td colspan="8">剩余长度</td>
    </tr>
  </tbody>
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
    <tr>
      <td>MQTT包</td>
      <td>控制标志</td>
      <td>Bit 3</td>
      <td>Bit 2</td>
      <td>Bit 1</td>
      <td>Bit 0</td>
    </tr>
  </thead>
  <tbody>
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
  </tbody>
</table>

DUP = 重复发送的 PUBLISH 包
QoS = PUBLISH 包的服务质量标识
RETAIN = PUBLISH 保留消息标识

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

### 2.2.2 属性集

在 CONNECT，CONNACK，PUBLISH，PUBACK，PUBREC，PUBREL，PUBCOMP，SUBSCRIBE，SUBACK，UNSUBSCRIBE，UNSUBACK，DISCONNECT 和 AUTH 包的可变头中的最后一个字段是属性集。在 CONNECT 的载荷中也存在着一组可选的遗嘱属性集。

属性集由属性长度和属性组成。

#### 2.2.2.1 属性长度

属性长度是一个变长整数。属性长度的值不包括自己所占用的字节数，但包括了后续所有属性占用的字节数。<span class="vcMarked">如果没有属性，必须明确通过一个0值的属性长度来表示</span> <span class="vcReferred">[MQTT-2.2.2-1]</span>。

#### 2.2.2.2 属性

属性由一个标识了其用途和数据类型的ID和一个后续的值组成。属性ID是一个变长整数。当一个数据包使用的属性ID和其包类型不一致，或属性的值和ID指明的类型不一致时，视为一个格式错误的包。如果收到，需使用带有原因码为 0x81 的 CONNACK 或 DISCONNECT 数据包，采用 [4.13](#4-13-错误处理) 描述的方法处理此错误。不同ID的属性没有顺序要求。

表2-4 属性
<table>
  <thead>
    <tr>
      <th colspan="2">ID</th>
      <th rowspan="2">名称（用途）</th>
      <th rowspan="2">数据类型</th>
      <th rowspan="2">包类型 / 遗嘱属性</th>
    </tr>
    <tr>
      <th>十进制</th><th>十六进制</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>0x01</td><td>载荷格式标识</td><td>单字节</td><td>PUBLISH，Will Properties</td></tr>
    <tr><td>2</td><td>0x02</td><td>消息过期间隔</td><td>4字节整数</td><td>PUBLISH，Will Properties</td></tr>
    <tr><td>3</td><td>0x03</td><td>内容类型</td><td>UTF-8字符串</td><td>PUBLISH，Will Properties</td></tr>
    <tr><td>8</td><td>0x08</td><td>响应主题</td><td>UTF-8字符串</td><td>PUBLISH，Will Properties</td></tr>
    <tr><td>9</td><td>0x09</td><td>关联数据</td><td>二进制数据</td><td>PUBLISH，Will Properties</td></tr>
    <tr><td>11</td><td>0x0B</td><td>订阅ID</td><td>变长整数</td><td>PUBLISH，SUBSCRIBE</td></tr>
    <tr><td>17</td><td>0x11</td><td>会话过期间隔</td><td>4字节整数</td><td>CONNECT，CONNACK，DISCONNECT</td></tr>
    <tr><td>18</td><td>0x12</td><td>分配的客户端ID</td><td>UTF-8字符串</td><td>CONNACK</td></tr>
    <tr><td>19</td><td>0x13</td><td>服务端保活时间</td><td>2字节整数</td><td>CONNACK</td></tr>
    <tr><td>21</td><td>0x15</td><td>认证方式</td><td>UTF-8字符串</td><td>CONNECT，CONNACK，AUTH</td></tr>
    <tr><td>22</td><td>0x16</td><td>认证数据</td><td>二进制数据</td><td>CONNECT，CONNACK，AUTH</td></tr>
    <tr><td>23</td><td>0x17</td><td>请求问题信息</td><td>单字节</td><td>CONNECT</td></tr>
    <tr><td>24</td><td>0x18</td><td>遗嘱延迟间隔</td><td>4字节整数</td><td>Will Properties</td></tr>
    <tr><td>25</td><td>0x19</td><td>请求响应信息</td><td>单字节</td><td>CONNECT</td></tr>
    <tr><td>26</td><td>0x1A</td><td>响应信息</td><td>UTF-8字符串</td><td>CONNACK</td></tr>
    <tr><td>28</td><td>0x1C</td><td>服务引用</td><td>UTF-8字符串</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>31</td><td>0x1F</td><td>原因字符串</td><td>UTF-8字符串</td><td>CONNACK，PUBACK，PUBREC，PUBREL，PUBCOMP，SUBACK，UNSUBACK，DISCONNECT，AUTH</td></tr>
    <tr><td>33</td><td>0x21</td><td>接收最大值</td><td>2字节整数</td><td>CONNECT，CONNACK</td></tr>
    <tr><td>34</td><td>0x22</td><td>主题别名最大值</td><td>2字节整数</td><td>CONNECT，CONNACK</td></tr>
    <tr><td>35</td><td>0x23</td><td>主题别名</td><td>2字节整数</td><td>PUBLISH</td></tr>
    <tr><td>36</td><td>0x24</td><td>QoS最大值</td><td>单字节</td><td>CONNACK</td></tr>
    <tr><td>37</td><td>0x25</td><td>保留消息可用</td><td>单字节</td><td>CONNACK</td></tr>
    <tr><td>38</td><td>0x26</td><td>用户属性</td><td>UTF-8字符串对</td><td>CONNECT，CONNACK，PUBLISH，Will Properties，PUBACK，PUBREC，PUBREL，PUBCOMP，SUBSCRIBE，SUBACK，UNSUBSCRIBE，UNSUBACK，DISCONNECT，AUTH</td></tr>
    <tr><td>39</td><td>0x27</td><td>最大包尺寸</td><td>4字节整数</td><td>CONNECT，CONNACK</td></tr>
    <tr><td>40</td><td>0x28</td><td>通配符订阅可用</td><td>单字节</td><td>CONNACK</td></tr>
    <tr><td>41</td><td>0x29</td><td>订阅ID可用</td><td>单字节</td><td>CONNACK</td></tr>
    <tr><td>42</td><td>0x2A</td><td>共享订阅可用</td><td>单字节</td><td>CONNACK</td></tr>
  </tbody>
</table>

*非规范性评论*

*虽然属性ID被定义为一个变长整数，但在规范的此版本中所有的属性ID都只有一个字节的长度。*

## 2.3 载荷

有些 MQTT 包的尾部是载荷。在 PUBLISH 包中的载荷就是应用消息。

表2-5 包含载荷的 MQTT 包

| MQTT 包 | 载荷 |
| --- | --- |
| CONNECT | 有 |
| CONNACK | 无 |
| PUBLISH | 可选 |
| PUBACK | 无 |
| PUBREC | 无 |
| PUBREL | 无 |
| PUBCOMP | 无 |
| SUBSCRIBE | 有 |
| SUBACK | 有 |
| UNSUBSCRIBE | 有 |
| UNSUBACK | 有 |
| PINGREQ | 无 |
| PINGRESP | 无 |
| DISCONNECT | 无 |
| AUTH | 无 |

## 2.4 原因码

原因码是一个单字节的无符号整数值，用来表示一个操作的结果。小于 0x80 的原因码用来表示操作成功，最常见的表示成功的原因码是 0x00。0x80或更大的原因码表示失败。

在 CONNACK，PUBACK，PUBREC，PUBREL，PUBCOMP，DISCONNECT 和 AUTH 包的可变头中有一个原因码字段。在 SUBACK 和 UNSUBACK 的载荷中包一个列表，其中有一个或多个原因码字段。

原因码字段值的通用定义如下：

表2-6 原因码

<table>
  <thead>
    <tr>
      <th colspan="2">原因码</th>
      <th rowspan="2">名称</th>
      <th rowspan="2">包类型</th>
    </tr>
    <tr>
      <th>十进制</th><th>十六进制</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>0</td><td>0x00</td><td>成功</td><td>CONNACK，PUBACK，PUBREC，PUBREL，PUBCOMP，UNSUBACK，AUTH</td></tr>
    <tr><td>0</td><td>0x00</td><td>普通断开</td><td>DISCONNECT</td></tr>
    <tr><td>0</td><td>0x00</td><td>授予 QoS 0</td><td>SUBACK</td></tr>
    <tr><td>1</td><td>0x01</td><td>授予 QoS 1</td><td>SUBACK</td></tr>
    <tr><td>2</td><td>0x02</td><td>授予 QoS 2</td><td>SUBACK</td></tr>
    <tr><td>4</td><td>0x04</td><td>携带遗嘱的断开链接</td><td>DISCONNECT</td></tr>
    <tr><td>16</td><td>0x16</td><td>没有匹配的订阅者</td><td>PUBACK，PUBREC</td></tr>
    <tr><td>17</td><td>0x11</td><td>没有存在的订阅</td><td>UNSUBACK</td></tr>
    <tr><td>24</td><td>0x18</td><td>继续认证</td><td>AUTH</td></tr>
    <tr><td>25</td><td>0x19</td><td>重新认证</td><td>AUTH</td></tr>
    <tr><td>128</td><td>0x80</td><td>未指定错误</td><td>CONNACK，PUBACK，PUBREC，SUBACK，UNSUBACK，DISCONNECT</td></tr>
    <tr><td>129</td><td>0x81</td><td>格式错误的包</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>130</td><td>0x82</td><td>协议错误</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>131</td><td>0x83</td><td>特定实现错误</td><td>CONNACK，PUBACK，PUBREC，SUBACK，UNSUBACK，DISCONNECT</td></tr>
    <tr><td>132</td><td>0x84</td><td>协议版本不支持</td><td>CONNACK</td></tr>
    <tr><td>133</td><td>0x85</td><td>客户端ID不可用</td><td>CONNACK</td></tr>
    <tr><td>134</td><td>0x86</td><td>用户名或密码错误</td><td>CONNACK</td></tr>
    <tr><td>135</td><td>0x87</td><td>未经授权</td><td>CONNACK，PUBACK，PUBREC，SUBACK，UNSUBACK，DISCONNECT</td></tr>
    <tr><td>136</td><td>0x88</td><td>服务器不可用</td><td>CONNACK</td></tr>
    <tr><td>137</td><td>0x89</td><td>服务器忙</td><td>CONNACK</td></tr>
    <tr><td>138</td><td>0x8A</td><td>被禁止</td><td>CONNACK</td></tr>
    <tr><td>139</td><td>0x8B</td><td>服务器关闭</td><td>DISCONNECT</td></tr>
    <tr><td>140</td><td>0x8C</td><td>认证模式错误</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>141</td><td>0x8D</td><td>保活超时</td><td>DISCONNECT</td></tr>
    <tr><td>142</td><td>0x8E</td><td>会话被接管</td><td>DISCONNECT</td></tr>
    <tr><td>143</td><td>0x8F</td><td>主题过滤器不可用</td><td>SUBACK，UNSUBACK，DISCONNECT</td></tr>
    <tr><td>144</td><td>0x90</td><td>主题名不可用</td><td>CONNACK，PUBACK，PUBREC，DISCONNECT</td></tr>
    <tr><td>145</td><td>0x91</td><td>包ID已被使用</td><td>PUBACK，PUBREC，SUBACK，UNSUBACK</td></tr>
    <tr><td>146</td><td>0x92</td><td>包ID未找到</td><td>PUBREL，PUBCOMP</td></tr>
    <tr><td>147</td><td>0x93</td><td>超出接收最大值</td><td>DISCONNECT</td></tr>
    <tr><td>148</td><td>0x94</td><td>主题别名不可用</td><td>DISCONNECT</td></tr>
    <tr><td>149</td><td>0x95</td><td>包过大</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>150</td><td>0x96</td><td>消息频率过高</td><td>DISCONNECT</td></tr>
    <tr><td>151</td><td>0x97</td><td>超限</td><td>CONNACK，PUBACK，PUBREC，SUBACK，DISCONNECT</td></tr>
    <tr><td>152</td><td>0x98</td><td>管理员行为</td><td>DISCONNECT</td></tr>
    <tr><td>153</td><td>0x99</td><td>载荷格式错误</td><td>CONNACK，PUBACK，PUBREC，DISCONNECT</td></tr>
    <tr><td>154</td><td>0x9A</td><td>不支持保留消息</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>155</td><td>0x9B</td><td>不支持 QoS</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>156</td><td>0x9C</td><td>临时使用另一台服务器</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>157</td><td>0x9D</td><td>服务器迁移</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>158</td><td>0x9E</td><td>不支持共享订阅</td><td>SUBACK，DISCONNECT</td></tr>
    <tr><td>159</td><td>0x9F</td><td>连接频率超限</td><td>CONNACK，DISCONNECT</td></tr>
    <tr><td>160</td><td>0xA0</td><td>最大连接时间</td><td>DISCONNECT</td></tr>
    <tr><td>161</td><td>0xA1</td><td>不支持订阅ID</td><td>SUBACK，DISCONNECT</td></tr>
    <tr><td>162</td><td>0xA2</td><td>不支持通配符订阅</td><td>SUBACK，DISCONNECT</td></tr>
  </tbody>
</table>

*非规范性评论*

*对于 0x91（包ID已被使用）的原因码，对此的处理应是处理重复状态，或是使用 Clean Start 为 1 的标识重新创建连接来重置会话，或是检查客户端或服务端的实现是否有缺陷。*

# 3 MQTT包

## 3.1 CONNECT - 连接请求

<span class="vcMarked">当客户端和服务器的网络连接建立后，客户端向服务器发送的第一个数据包必须是 CONNECT 包</span> <span class="vcReferred">[MQTT-3.1.0-1]</span>。

一个客户端在一次网络连接中只能发送一个 CONNECT 包。<span class="vcMarked">服务器必须将客户端发送的第二个 CONNECT 包视为协议错误并关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.0-2]</span>。参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

CONNECT 的载荷包含一个或更多的字段，包括唯一的客户端ID，遗嘱主题，遗嘱载荷，用户名和密码。除了客户端ID以外的字段都可以省略，他们的存在与否根据可变头中的标识确定。

### 3.1.1 CONNECT固定头

图3‑1 CONNECT 包固定头

<table>
  <thead>
    <tr><td>Bit</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td>byte 1</td><td colspan="4">MQTT包类型（1）</td><td colspan="4">保留</td></tr>
    <tr><td></td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
    <tr><td>byte 2...</td><td colspan="8">剩余长度</td></tr>
  </tbody>
</table>

**剩余长度**

表示可变头长度和载荷长度的总和，使用变长整数表示。

### 3.1.2 CONNECT可变头

CONNECT 包中的可变头按固定顺序提供下列字段：协议名、协议版本、连接标识、保活时间和属性集。属性集的编码方式请参考 [2.2.2](#2-2-2-属性集)。

#### 3.1.2.1 协议名

图3-2 协议名字节

<table>
  <thead>
    <tr><td></td><td>描述</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td colspan="10">协议名</td></tr>
    <tr><td>byte 1</td><td>长度高位（MSB）</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
    <tr><td>byte 2</td><td>长度低位（LSB）</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td></tr>
    <tr><td>byte 3</td><td>'M'</td><td>0</td><td>1</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td></tr>
    <tr><td>byte 4</td><td>'Q'</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td></tr>
    <tr><td>byte 5</td><td>'T'</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td></tr>
    <tr><td>byte 6</td><td>'T'</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td></tr>
  </tbody>
</table>

协议名是 UTF-8字符串表示的大写 “MQTT”，就像上图所示。这个字符串、他的位置和他的长度在未来的 MQTT 规范版本中永远不会改变。

支持多种协议的服务器可以通过协议名称来判断收到的数据是否是 MQTT 数据。<span class="vcMarked">协议名称必须是 UTF-8字符串表示的 “MQTT”。如果服务器不想接收此连接，同时又想告知客户端服务器是一个 MQTT 服务器，可以发送一个带有 0x84（协议版本不支持）原因码的 CONNACK，随后服务端必须关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.2-1]</span>。
 
*非规范性评论*

*数据包检查器（例如防火墙）可以使用协议名称来识别 MQTT 流量。*

#### 3.1.2.2 协议版本

图3‑3 协议版本字节

<table>
  <thead>
    <tr><td></td><td>描述</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td colspan="10">协议版本</td></tr>
    <tr><td>byte 7</td><td>版本（5）</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td></tr>
  </tbody>
</table>

此处的单字节无符号值表示了客户端使用的协议的版本。MQTT 5.0 版本的协议版本的值应为 5（0x05）。

支持多个协议版本的服务器可以通过协议版本字段来判断客户端使用何种版本的 MQTT 协议。<span class="vcMarked">如果客户端使用的协议版本不为 5 而且服务器不想接受此 CONNECT 包，服务器可以发送一个带有 0x84（协议版本不支持）原因码的 CONNACK，随后服务端必须关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.2-2]</span>。

#### 3.1.2.3 连接标识

连接标志字节包含了几个指定 MQTT 连接行为的参数。他还用来指示载荷中的某些字段是否存在。

图3‑4 连接标识位

<table>
  <thead>
    <tr><td>Bit</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td></td><td>用户名标识</td><td>密码标识</td><td>遗嘱保留消息</td><td colspan="2">遗嘱QoS</td><td>遗嘱标识</td><td>全新开始</td><td>保留</td></tr>
    <tr><td>byte 8</td><td>X</td><td>X</td><td>X</td><td>X</td><td>X</td><td>X</td><td>X</td><td>0</td></tr>
  </tbody>
</table>

<span class="vcMarked">服务器必须验证 CONNECT 包中的保留位的值是 0</span> <span class="vcReferred">[MQTT-3.1.2-3]</span>。如果保留位的值非 0，则视为一个格式错误的包，参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

#### 3.1.2.4 全新开始

**位置：**连接标识字节中的比特位 1。

这个比特位指定了此连接是一个全新的会话还是一个已经存在会话的延续。参考 [4.1](#4-1-会话状态) 了解关于会话状态的定义。

<span class="vcMarked">如果接收到全新开始值置为 1 的 CONNECT 包，客户端和服务器必须丢弃任何已经存在的会话并开始一个新的会话</span> <span class="vcReferred">[MQTT-3.1.2-4]</span>。因此，当 CONNECT 包中的全新开始值置为 1 时，对应的 CONNACK 包中的会话存在总是会被至为0。

<span class="vcMarked">如果服务器接收到的 CONNECT 包中的全新开始被置为 0 并且服务器中已经存在和客户端ID关联的会话，服务器必须基于已经存在的会话状态恢复客户端的连接</span> <span class="vcReferred">[MQTT-3.1.2-5]</span>。<span class="vcMarked">如果服务端接收到的 CONNECT 包中的全新开始被置为 0 并且服务器中没有和客户端ID关联的会话，服务器必须创建一个新的会话</span> <span class="vcReferred">[MQTT-3.1.2-6]</span>。

#### 3.1.2.5 遗嘱标识

**位置：**连接标识字节中的比特位 2。

<span class="vcMarked">如果遗嘱标识被置为 1，则表示遗嘱消息必须被存储在服务器中，并且关联到此会话</span> <span class="vcReferred">[MQTT-3.1.2-7]</span>。遗嘱消息由 CONNECT 载荷中的遗嘱属性集、遗嘱主题和遗嘱载荷组成。<span class="vcMarked">遗嘱消息必须在网络连接断开后的遗嘱延迟间隔时间过期后或会话结束时发布，除非由于服务器接收到一个带有 0x00（普通断开）原因码的 DISCONNECT 包从而删除了遗嘱消息，或是在遗嘱延迟间隔时间过期前接收了一个带有相同客户端ID的连接</span> <span class="vcReferred">[MQTT-3.1.2-8]</span>。

遗嘱消息被发布的场景包括不限于：

- 服务端检测到了 I/O 错误或网络故障
- 客户端没有成功在保活时间内通信
- 客户端在没有发送原因码为 0x00（普通断开）的 DISCONNECT 包的前提下断开网络连接
- 服务器在没有收到原因码为 0x00（普通断开）的 DISCONNECT 包的前提下断开网络连接

<span class="vcMarked">当遗嘱标识被置为 1 时，载荷中必须包括遗嘱属性集、遗嘱主题和遗嘱载荷字段</span> <span class="vcReferred">[MQTT-3.1.2-9]</span>。<span class="vcMarked">当服务器发布遗嘱后或服务器从客户端收到了原因码为 0x00（普通断开）的 DISCONNECT 包后，服务器必须从会话状态中删除遗嘱消息</span> <span class="vcReferred">[MQTT-3.1.2-10]</span>。

服务器应该在下列两种情况中的某一种先发生时发布遗嘱消息：网络连接断开后经过了遗嘱延迟间隔时间、会话结束。如果发生了服务器关闭或故障，服务器也许可以在随后的重启之后发布遗嘱消息。当此种情况发生时，服务器故障发生的时间和遗嘱消息发布的时间之间可能会有延迟。

参考 [3.1.3.2](#3-1-3-2-遗嘱属性集) 了解关于遗嘱延迟间隔的消息。

*非规范性评论*

*客户端可以设置遗嘱延迟间隔大于会话过期间隔，再发送带有原因码 0x04（携带遗嘱的断开链接）的 DISCONNECT 断开连接。这样可以使用遗嘱消息来通知会话已过期。*

#### 3.1.2.6 遗嘱QoS

**位置：**连接标识字节中的比特位 4 和 3。

这两个比特位指定了发布遗嘱消息时使用的 QoS 等级。

<span class="vcMarked">当遗嘱标识被置为 0 时，遗嘱QoS必须被置为 0（0x00）</span> <span class="vcReferred">[MQTT-3.1.2-11]</span>。

<span class="vcMarked">当遗嘱标识被置为 1 时，遗嘱QoS的值可以是 0（0x00），1（0x01）或 2（0x02）</span> <span class="vcReferred">[MQTT-3.1.2-12]</span>。当值为 3（0x03）时视为格式错误的包，参考 [4.13](#4-13-错误处理) 查看关于错误处理的信息。

#### 3.1.2.7 遗嘱保留消息

**位置：**连接标识字节中的比特位 5。

这个比特位指定了当遗嘱消息发布时是否会被保留。

<span class="vcMarked">当遗嘱标识被置为 0 时，遗嘱保留消息的值必须被置为 0</span> <span class="vcReferred">[MQTT-3.1.2-13]</span>。<span class="vcMarked">当遗嘱标识被置为 1 且遗嘱保留消息被置为 0 时，服务器必须将遗嘱消息作为一个非保留消息发布</span> <span class="vcReferred">[MQTT-3.1.2-14]</span>。<span class="vcMarked">当遗嘱标识被置为 1 且遗嘱保留消息被置为 1 时，服务器必须将遗嘱消息作为一个保留消息发布</span> <span class="vcReferred">[MQTT-3.1.2-15]</span>。

#### 3.1.2.8 用户名标识

**位置：**连接标识字节中的比特位 7。

<span class="vcMarked">当用户名标识被置为 0 时，载荷中必须不能存在用户名</span> <span class="vcReferred">[MQTT-3.1.2-16]</span>。<span class="vcMarked">当用户名标识被置为 1 时，载荷中必须存在用户名</span> <span class="vcReferred">[MQTT-3.1.2-17]</span>。

#### 3.1.2.9 密码标识

**位置：**连接标识字节中的比特位 6。

<span class="vcMarked">当密码标识被置为 0 时，载荷中必须不能存在密码</span> <span class="vcReferred">[MQTT-3.1.2-18]</span>。<span class="vcMarked">当密码标识被置为 1 时，载荷中必须存在密码</span> <span class="vcReferred">[MQTT-3.1.2-19]</span>。

*非规范性评论*

*本版协议允许在不使用用户名时使用密码，和 MQTT v3.1.1 不同。这反映了密码作为密码以外的凭证的常见用途。*

#### 3.1.2.10 保活时间

图3-5 保活时间字节

<table>
  <thead>
    <tr><td>Bit</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td>byte 9</td><td colspan="8">保活时间高位（MSB）</td></tr>
    <tr><td>byte 10</td><td colspan="8">保活时间地位（LSB）</td></tr>
  </tbody>
</table>

保活时间是一个表示时间间隔秒数的 2字节整数。他是允许客户端在发送一个 MQTT 包后到发送下一个数据包之间的最大时间间隔。客户端有责任确保两个 MQTT 包之间的时间间隔不超过保活时间。<span class="vcMarked">如果保活时间不为 0 且没有任何其他需要发送的数据包，客户端必须发送 PINGREQ 包</span> <span class="vcReferred">[MQTT-3.1.2-20]</span>。

<span class="vcMarked">如果服务端在 CONNACK 中提供了服务端保活时间，则客户端必须采用服务端保活时间的值来替代自己发送的保活时间的值</span> <span class="vcReferred">[MQTT-3.1.2-21]</span>。

不论保活时间的值如何设置，客户端可以在任何时间发送 PINGREQ，并通过检查相应的 PINGRESP 来确认服务端与网络是否可用。

<span class="vcMarked">如果保活时间为非零值且服务器在 1.5 倍的保活时间内没有收到来自客户端的任何 MQTT 包，服务器必须断开到客户端的网络连接并视为网络连接故障</span> <span class="vcReferred">[MQTT-3.1.2-22]</span>。

如果客户端在发送 PINGREQ 的合理时间后依然没有收到 PINGRESP，客户端应断开到服务器的网络连接。

保活时间的值为 0 表示关闭保活机制。当保活时间为 0 时客户端没有义务按任何特定时间发送 MQTT 包。

*非规范性评论*

*服务器可能会因为其他原因关闭向客户端的连接，例如服务器关机。设置保活时间并非意味着客户端可以持续保持连接。*

*非规范性评论*

*保活时间的具体值是由应用程序设置的，通常来说，是几分钟。保活时间的最大值 65535 表示 18小时12分钟15秒。*

#### 3.1.2.11 CONNECT 属性集

##### 3.1.2.11.1 属性长度

CONNECT 包可变头中的属性集长度，使用变长整数编码。

##### 3.1.2.11.2 会话过期间隔

会话过期间隔的属性ID是**17 (0x11) Byte**。

随后跟随 4字节整数 用来表示会话过期间隔，单位为秒。在属性集中出现超过一次会话过期间隔视为协议错误。

当没有设置会话过期间隔是使用 0 作为默认值。如果会话过期间隔的值为 0 或者没有设置，当网络连接断开时会话立即结束。

如果会话过期间隔被设置为 0xFFFFFFFF (UINT_MAX)，表示会话不会过期。

<span class="vcMarked">当会话过期间隔的值大于0时，客户端和服务器都必须在网络连接断开后存储会话状态</span> <span class="vcReferred">[MQTT-3.1.2-23]</span>。

*非规范性评论*

*客户端或服务器的时钟可能在某些时间内没有运行，例如当客户端或服务器被关闭时。这可能会导致状态被延迟删除。*

参考 [4.1](#4-1-会话状态) 了解更多关于会话的信息。参考 [4.1.1](#4-1-1-存储会话状态) 了解更多关于会话状态存储的细节和限制。

当会话过期时，客户端与服务器不需要自动删除会话状态。

*非规范性评论*

*将全新开始标识设置为 1 并将会话过期间隔设置为 0，等同于在 MQTT v3.1.1 规范中将 CleanSession 设置为 1。将全新开始标识设置为 0 且不设置会话过期间隔，等同于在 MQTT v3.1.1 规范中将 CleanSession 设置为 0。*

*非规范性评论*

*一个只想处理其在线时消息的客户端可以将全新开始标识设置为 1 同时将会话过期间隔设置为 0。这样他将不会收到任何在他离线时产生的消息，并且他必须在每次连接后重新订阅所有他需要的主题。*

*非规范性评论*

*当客户端使用一个间歇性可用的网络连接服务器时，客户端可以设置一个较短的会话过期间隔这样每当网络连接可用时他都会重连并继续进行可靠的消息传递。而当客户端没有重连时，允许会话超时，这样应用消息就会丢失。*

*非规范性评论*

*当一个客户端使用较长的会话过期间隔连接服务器时，这意味着他希望服务器能够在他断开连接后的较长时间里维护会话状态。客户端只有在明确的意识到自己会在某个时间点重连时才使用这种较长的会话过期间隔。当一个客户端决定未来不再使用此会话，他应该在断开时将会话过期间隔设置为0。*

*非规范性评论*

*客户端总是应该基于 CONNACK 中的会话存在标识来判断服务器是否保存有该客户端的会话状态。*

*非规范性评论*

*客户端可以依赖服务器返回的会话存在标识来判断会话是否过期，而不必自己实现会话过期机制。如果客户端自己实现了会话过期机制，那么则必须在自己的会话状态中记录何时该会话状态需要被删除。*

##### 3.1.2.11.3 接收最大值

接收最大值的属性ID是**33 (0x21) Byte**。

随后跟随 2字节整数 用来表示有状态数据接收的最大值。接收最大值在属性集中出现超过一次，或接收最大值的值为0，均为协议错误。

客户端使用此值限制他同时处理的 QoS1 和 QoS2 包发布动作数量。没有任何机制限制服务端可能尝试的对 QoS0 包的发布。

接受最大值的值仅对当前网络连接有效。如果没有设置接收最大值那么他的默认值是 65535。

参考 [4.9 流量控制](#4-9-流量控制) 了解关于接收最大值的使用细节。

##### 3.1.2.11.4 最大包尺寸

最大包尺寸的属性ID是**33 (0x21) Byte**。

随后跟随 4字节整数 用来表示客户端可以接受的最大包尺寸。如果没有设置最大包尺寸，则其受限于固定头中的剩余长度限制，除此外并没有其他限制。

在属性集中出现超过一次最大包尺寸或其值为 0 均视为协议错误。

*非规范性评论*

*当包的尺寸需要被限制时，应用程序有责任选择一个合适的最大包尺寸的值。*

最大包尺寸表示 **MQTT 包完整的字节数**，其定义参考 [2.1.4](#2-1-4-剩余长度)。客户端使用最大包尺寸告知服务端，客户端不会处理超过此尺寸限制的信息。

<span class="vcMarked">服务端必须不向客户端发送超过最大包尺寸的数据包</span> <span class="vcReferred">[MQTT-3.1.2-24]</span>。如果客户端收到了超过其最大包尺寸限制的数据报，这被视为一个协议错误，客户端需要使用带有原因码 0x95（包过大）的 DISCONNECT 包来中断连接，参考 [4.13](#4-13-错误处理) 的描述。

<span class="vcMarked">当一个包因超过最大包尺寸而无法发送，服务器必须将其丢弃，并视为发送成功</span> <span class="vcReferred">[MQTT-3.1.2-25]</span>。

当某个包的尺寸大于共享订阅中的部分客户端的最大包尺寸，而又可以被另外的某些客户端接收时，服务器可以决定丢弃此包或者将其发送到可以接收此包的客户端。

*非规范性评论*

*当一个包未被发送即被丢弃时，服务器可以将其放入“丢包队列”或者提供其他的诊断机制。此类行为实现不属于本规范的范畴。*

##### 3.1.2.11.5 主题别名最大值

主题别名最大值的属性ID是**34 (0x22) Byte**。

随后跟随 2字节整数 表示主题别名的最大值。在属性集中出现超过一次主题别名最大值视为协议错误。如果主题别名最大值没有设置，则采用默认值 0。

主题别名最大值表示了客户端可以接受的来自服务器发送的主题别名的最大数量。客户端使用此值来约束他在本次连接中可以持有的主题别名数量。<span class="vcMarked">服务器必须不能发送一个主题别名的值大于客户端设置的主题别名最大值的 PUBLISH 包</span> <span class="vcReferred">[MQTT-3.1.2-26]</span>。主题别名的值为 0 表示客户端在本次连接中不支持任何主题别名。<span class="vcMarked">如果主题别名最大值未设置或值为 0，服务器必须不向客户端发送主题别名</span> <span class="vcReferred">[MQTT-3.1.2-27]</span>。

##### 3.1.2.11.6 请求响应信息

请求响应信息的属性ID是**25 (0x19) Byte**。

随后跟随一个值为 0 或 1 的字节。在属性集中出现超过一次请求响应信息，或其值不为 1 或 0，均视为协议错误。如果请求响应信息没有设置，则采用默认值 0。

客户端通过此值请求服务器，希望服务器在 CONNACK 中回复响应信息。<span class="vcMarked">此值为 0 表示服务器必须不能在 CONNACK 中回复响应信息</span> <span class="vcReferred">[MQTT-3.1.2-28]</span>。此值为 1 表示服务器可以在 CONNACK 中回复响应信息。

*非规范性评论*

*即使客户端请求了，服务器也可以不在 CONNACK 中回复响应信息。*

参考 [4.10](#4-10-请求-响应) 了解关于 请求/响应 的更多信息。

##### 3.1.2.11.7 请求问题信息

请求问题信息的属性ID是**23 (0x17) Byte**。

随后跟随一个值为 0 或 1 的字节。在属性集中出现超过一次请求问题信息，或其值不为 1 或 0，均视为协议错误。如果请求问题信息没有设置，则采用默认值 0。

客户端通过此值表示服务器是是否应该在故障时发送原因字符串或用户属性。

<span class="vcMarked">如果请求问题信息的值为 0，服务器可以在 CONNACK 或 DISCONNECT 包中携带原因字符串或用户属性，但必须不在除 PUBLISH，CONNACK，DISCONNECT 之外的包中携带原因字符串或用户属性</span> <span class="vcReferred">[MQTT-3.1.2-29]</span>。如果请求问题信息的值为 0 且客户端接收到除 PUBLISH，CONNACK，DISCONNECT 之外的包中带有了原因字符串或用户属性，客户端使用带有原因码 0x82（协议错误）的 DISCONNECT 包断开连接，参考 [4.13](#4-13-错误处理) 了解更多信息。

如果请求问题信息的值为 1，服务器可以在任何允许的包中返回原因字符串或用户属性。

##### 3.1.2.11.8 用户属性

用户属性的属性ID是**38 (0x26) Byte**。

随后跟随 UTF-8字符串对。

用户属性可以出现多次，用来携带多个 键-值 对。同样的 键 允许出现超过一次。

*非规范性评论*

CONNECT 包中的用户属性可以用来从客户端向服务器发送连接过程中依赖的属性。这些属性的含义超出了本规范的范围。

##### 3.1.2.11.9 认证方式

认证方式的属性ID是**21 (0x15) Byte**。

随后跟随 UTF-8字符串，其中包括了使用的扩展认证方式的名称。认证方式在属性集中出现超过一次视为协议错误。

如果没有设置认证方式，将不会执行扩展认证。参考 [4.12](#4-12-增强认证)。

<span class="vcMarked">如果客户端再 CONNECK 包中设置了认证方式，那么在其收到 CONNACK 包之前，客户端必须不能发送除了 AUTH 和 DISCONNECT 包之外的任何类型的包</span> <span class="vcReferred">[MQTT-3.1.2-30]</span>。

##### 3.1.2.11.10 认证数据

认证数据的属性ID是**22 (0x16) Byte**。

随后跟随 二进制数据 其中包括认证数据。当认证方式不存在是提供认证数据，或是在任何情况下提供超过一次的认证数据，均视为协议错误。

认证数据的内容由认证方式决定。参考 [4.12](#4-12-增强认证) 了解更多关于扩展认证的信息。

#### 3.1.2.12 可变头非规范性示例

图3-6 可变头示例

<table>
  <thead>
    <tr><td></td><td>描述</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td colspan="10">协议名称</td></tr>
    <tr>
      <td>byte 1</td><td>长度高字节（MSB）（0）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 2</td><td>长度低字节（LSB）（4）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 3</td><td>‘M’</td>
      <td>0</td><td>1</td><td>0</td><td>0</td><td>1</td><td>1</td><td>0</td><td>1</td>
    </tr>
    <tr>
      <td>byte 4</td><td>‘Q’</td>
      <td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td>
    </tr>
    <tr>
      <td>byte 5</td><td>‘T’</td>
      <td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 6</td><td>‘T’</td>
      <td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td>
    </tr>
    <tr><td colspan="10">协议版本</td></tr>
    <tr>
      <td>byte 7</td><td>版本（5）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td>
    </tr>
    <tr><td colspan="10">连接标识</td></tr>
    <tr>
      <td>byte 8</td>
      <td>
        用户名标识（1）</br>
        密码标识（1）</br>
        遗嘱保留消息（0）</br>
        遗嘱QoS（01）</br>
        遗嘱标识（1）</br>
        全新开始（1）</br>
        保留（0）
      </td>
      <td>1</td><td>1</td><td>0</td><td>0</td><td>1</td><td>1</td><td>1</td><td>0</td>
    </tr>
    <tr><td colspan="10">保活时间</td></tr>
    <tr>
      <td>byte 9</td><td>保活时间高位（MSB）（0）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 10</td><td>保活时间低位（LSB）（10）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td>
    </tr>
    <tr><td colspan="10">属性集</td></tr>
    <tr>
      <td>byte 11</td><td>长度（5）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td>
    </tr>
    <tr>
      <td>byte 12</td><td>属性ID：会话过期间隔（17）</td>
      <td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td>
    </tr>
    <tr>
      <td>byte 13</td><td rowspan="4">会话过期间隔（10）</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 14</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 15</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td>
    </tr>
    <tr>
      <td>byte 16</td>
      <td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td>
    </tr>
  </tbody>
</table>

### 3.1.3 CONNECT载荷

<span class="vcMarked">CONNECT 中的载荷包含了一个或多个 长度 + 内容 格式的字段，这些字段的存在与否由可变头中的标志位决定。这些字段的顺序是固定的，如果存在的话，必须按照 客户端ID，遗嘱属性集，遗嘱主题，遗嘱载荷，用户名，密码 这样的顺序出现</span> <span class="vcReferred">[MQTT-3.1.3-1]</span>。

#### 3.1.3.1 客户端ID

客户端ID用来在服务器端区分不同的客户端。每个连向服务器的客户端都拥有一个唯一的客户端ID。<span class="vcMarked">客户端ID必须被客户端和服务器用于关联客户端和服务器之间的会话状态</span> <span class="vcReferred">[MQTT-3.1.3-2]</span>。参考 [4.1](#4-1-会话状态) 了解更多关于会话状态的信息。

<span class="vcMarked">客户端ID必须作为 CONNECT 包载荷中的第一个字段出现</span> <span class="vcReferred">[MQTT-3.1.3-3]</span>。

<span class="vcMarked">客户端ID必须被编码为一个 UTF-8 字符串</span>，该数据类型的定义在 [1.5.4](#1-5-4-UTF-8字符串) <span class="vcReferred">[MQTT-3.1.3-4]</span>。

<span class="vcMarked">服务器必须允许客户端ID是长度为 1 到 23 个字节之间的 UTF-8 字符串，且仅包含下列字符：“0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ”</span> <span class="vcReferred">[MQTT-3.1.3-5]</span>。

服务器可以允许客户端ID的长度超过23字节。服务器可以允许客户端ID的内容包含上述以外的内容。

<span class="vcMarked">服务器可以允许客户端传递长度为0的客户端ID，当此情况发生时，服务器必须将此情况作为一个特殊情况对待，并为客户端分配一个唯一的客户端ID</span> <span class="vcReferred">[MQTT-3.1.3-6]</span>。<span class="vcMarked">服务器之后必须正常处理此 CONNECT 包，就如同客户端本身携带了这个唯一的客户端ID一样，而且必须在 CONNACK 包中返回这个分配的客户端ID</span> <span class="vcReferred">[MQTT-3.1.3-7]</span>。

<span class="vcMarked">如果服务器拒绝了客户端ID，服务器可以使用一个带有原因码 0x85（客户端ID不可用）的 CONNACK 包作为对客户端 CONNECT 包的响应，如同 [4.13](#4-13-错误处理) 中描述的那样，之后服务器必须关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.3-8]</span>。

*非规范性评论*

*客户端实现可以提供一个方便的生成随机客户端ID的方法。使用这种方法的客户端应该注意避免创建长期存在的孤立会话。*

#### 3.1.3.2 遗嘱属性集

如果遗嘱标识被置为 1，载荷中的下一个字段会是遗嘱属性集。遗嘱属性集决定了当遗嘱消息被发布时所使用的应用消息属性集，还决定了何时发布遗嘱消息。遗嘱属性集包含了属性长度和属性集内容。

##### 3.1.3.2.1 属性长度

遗嘱属性集中的属性集长度是一个变长整数。

##### 3.1.3.2.2 遗嘱延迟间隔

遗嘱延迟间隔的属性ID是**24 (0x18) Byte**。

随后跟随 4字节整数，表示遗嘱延迟间隔时间，单位为秒。遗嘱延迟间隔在属性集中出现超过一次视为协议错误。如果遗嘱延迟间隔未设置，默认值为0，表示遗嘱发布前没有间隔时间。

服务器在遗嘱延迟间隔结束或是会话结束时发布遗嘱，这两种条件以先触发的为准。<span class="vcMarked">如果在遗嘱延迟间隔结束前，该会话被新的网络连接延续，服务器必须不不发遗嘱</span> <span class="vcReferred">[MQTT-3.1.3-9]</span>。

*非规范性评论*

*遗嘱延迟间隔的一个作用是，当使用周期性可用的网络进行通信时，客户端使用遗嘱延迟间隔可以在遗嘱发布前重新连接并使用会话，而非每次连接断开后都发布遗嘱。*

*非规范性评论*

*当客户端和服务器的网络连接存在时，如果客户端再次使用相同的客户端ID连接到服务器，现有网络连接的遗嘱消息会被发布，除非新连接将 全新开始 标识置为 0 且遗嘱延迟间隔的值大于 0。如果遗嘱延迟间隔的值为 0，遗嘱消息会因为原有网络连接的关闭而发布，如果 全新开始 标识为 1 ，遗嘱消息会因为原有会话的结束而发布。*

##### 3.1.3.2.3 载荷格式标识

载荷格式标识的属性ID是**1 (0x01) Byte**。

随后的值表示遗嘱载荷的格式：

- 0（0x00）Byte 表示遗嘱消息是未指定的字节流，这等同于没有发送载荷格式标识。
- 1（0x01）Byte 表示遗嘱消息是 UTF-8字符串。载荷中的 UTF-8 数据必须符合 [Unicode](#1.3-Unicode) 和 [RFC3629](#1.3-RFC3629) 规范。

载荷格式标识在属性集中出现超过一次视为协议错误。服务器可以验证遗嘱消息是否符合载荷格式标识指定的格式，如果不符合则发送一个带有原因码 0x99（载荷格式错误）的 CONNACK 报文，详情参考 4.13。

##### 3.1.3.2.4 消息过期间隔

消息过期间隔的属性ID是**2 (0x02) Byte**。

随后跟随 4字节整数 用来表示消息过期间隔。消息过期间隔在属性集中出现超过一次视为协议错误。

当消息过期间隔存在时，此四字节的整数表示遗嘱消息的存活时间（单位秒），同时也是服务器发送此遗嘱消息时使用的发布过期时间。
 
如果没有设置此值，服务器发布遗嘱消息时不设置消息过期时间。

##### 3.1.3.2.5 内容类型

内容类型的属性ID是**3 (0x03) Byte**。

随后跟随 UTF-8字符串，表示遗嘱消息的内容类型。内容类型在属性集中出现超过一次视为协议错误。内容类型的值由发送方和接收方的应用程序定义。

##### 3.1.3.2.6 响应主题

响应主题的属性ID是**8 (0x08) Byte**。

随后跟随 UTF-8字符串，作为响应消息的主题名称。响应主题在属性集中出现超过一次视为协议错误。响应主题的存在表示将遗嘱消息视为一个请求。

参考 [4.10](#4-10-请求-响应) 了解关于 请求/响应 的更多信息。

##### 3.1.3.2.7 关联数据

关联数据的属性ID是**9 (0x09) Byte**。

随后跟随 二进制数据。请求消息的发送者使用关联数据来识别接收到的响应消息针对哪个请求。关联数据在属性集中出现超过一次视为协议错误。如果关联数据未设置，请求方无需携带任何关联数据。

关联数据的值仅对请求消息的发送者和响应消息的接收者有意义。

参考 [4.10](#4-10-请求-响应) 了解关于 请求/响应 的更多信息。

##### 3.1.3.2.8 用户属性

用户属性的属性ID是**38 (0x26) Byte**。

随后跟随 UTF-8字符串对。用户属性允许出现多次来表示多个键值对。同样的键允许出现多次。

<span class="vcMarked">服务器必须在发布遗嘱消息时维持用户属性的顺序</span> <span class="vcReferred">[MQTT-3.1.3-10]</span>。

*非规范性评论*

*这个属性只是提供一种传输键值对的方式，键值对的含义和解释只有负责发送和接收该属性的应用程序了解。*

#### 3.1.3.3 遗嘱主题

如果遗嘱标识被置为 1，载荷中的下一个字段会是遗嘱主题。<span class="vcMarked">遗嘱主题必须是一个 UTF-8字符串</span>，参考 [1.5.4](#1-5-4-UTF-8字符串) 中的定义<span class="vcReferred">[MQTT-3.1.3-11]</span>。

#### 3.1.3.4 遗嘱载荷

如果遗嘱标识被置为 1，载荷中的下一个字段会是遗嘱载荷。如同 [3.1.2.5](#3-1-2-5-遗嘱标识) 的定义，遗嘱载荷是会被发布遗嘱主题中的应用消息。遗嘱载荷字段内含有二进制数据。

#### 3.1.3.5 用户名

如果用户名标识被置为 1，载荷中的下一个字段会是用户名。<span class="vcMarked">用户名必须是一个 UTF-8字符串</span>，参考 [1.5.4](#1-5-4-UTF-8字符串) 中的定义<span class="vcReferred">[MQTT-3.1.3-12]</span>。用户名可以被服务器用作认证和授权。

#### 3.1.3.6 密码

如果密码标识被置为 1，载荷中的下一个字段会是密码。密码字段内容是 二进制数据。虽然此字段被称为密码，但此字段实际上可以携带任何形式的凭据信息。

### 3.1.4 CONNECT动作

请注意，服务器可以在同一 TCP 端口或其他网络端点上支持多个协议（包括其他版本的 MQTT 协议）。如果服务器确定协议是 MQTT v5.0，则它会按如下方式验证连接尝试。

1. 如果服务器在客户端的网络连接建立后的一段合理时间内没有收到 CONNECT 包，服务器应该关闭网络连接。
2. <span class="vcMarked">服务器必须验证 CONNECT 包的格式符合 [3.1](#3-1-CONNECT-连接请求) 中的描述，如不符合则关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.4-1]</span>。服务器可以参考 4.13 在关闭网络连接前使用带有 0x80 或更高值的原因码的 CONNACK 通知客户端。
3. <span class="vcMarked">服务器可以检查 CONNECT 包中的内容是否满足更进一步的限制要求，并且应该进行认证和授权检查。如果其中任何检查失败，服务器必须关闭网络连接</span> <span class="vcReferred">[MQTT-3.1.4-2]</span>。在关闭网络连接前，服务器可以参考 [3.2](#3-2-CONNACK-–-连接回复) 和 [4.13](#4-13-错误处理) 发送一个带有 0x80 或更高值的原因码的符合情况的 CONNACK 包。

如果验证成功，服务器施行下列动作。

1. <span class="vcMarked">如果客户端ID代表了一个已经连接到服务器的客户端，服务器参考 [4.13](#4-13-错误处理) 发送一个带有原因码 0x8E（会话被接管）的 DISCONNECT 包到当前已有连接的客户端，且必须关闭当前已有连接客户端的网络连接</span> <span class="vcReferred">[MQTT-3.1.4-3]</span>。如果已有连接的客户端包含遗嘱，遗嘱的发布情况参考 3.1.2.5。

*非规范性评论*

*如果已有连接的遗嘱延迟间隔值为0且存在遗嘱消息，遗嘱消息会因为网络连接断开的原因发布。当已有连接的会话过期间隔值为0，或是新连接的全新开始标识被置为 1时，如果已有连接存在遗嘱消息，那么遗嘱消息会被发布，因为原有会话已经在接管时结束。*

2.  <span class="vcMarked">服务器必须参考 [3.1.2.4](#3-1-2-4-全新开始) 中的描述处理全新开始标识</span> <span class="vcReferred">[MQTT-3.1.4-4]</span>。
3.  <span class="vcMarked">服务器必须使用带有原因码为 0x00（成功）的 CONNACK 回复 CONNECT 包</span> <span class="vcReferred">[MQTT-3.1.4-5]</span>。

*非规范性评论*

*如果服务器用于处理任何形式的业务关键数据，建议执行身份验证和授权检查。如果这些检查通过，服务器会使用带有原因码 0x00（成功）的 CONNACK 回复。如果检查失败，建议服务器根本不发送 CONNACK，因为这可能会提醒潜在攻击者 MQTT 服务器的存在，并鼓励此类攻击者发起拒绝服务或密码猜测攻击。*

4. 启动消息传递和保活监控。

客户端可以在发送 CONNECT 包之后立刻发送其他 MQTT 包；客户端无需等待接收到来自服务器的 CONNACK 包。<span class="vcMarked">如果服务器决绝了 CONNECT，服务器必须不处理客户端在 CONNECT 包之后发送的任何除了 AUTH 以外的包</span> <span class="vcReferred">[MQTT-3.1.4-6]</span>。

*非规范性评论*

*客户端通常会等待 CONNACK 数据包，然而，客户端可以选择在接受 CONNACK 前发送其他 MQTT 数据包，这样做可能会简化客户端实现，因为客户端无需监管连接状态。客户端需要接受在其收到 CONNACK 前，如果服务器拒绝了连接，其发送的数据都不会被服务器处理。*

*非规范性评论*

*在接收 CONNACK 之前发送其他 MQTT 包的客户端将不知道服务器约束以及是否正在使用任何现有会话。*

*非规范性评论*
 
*如果客户端在认证完成前就发送了过多的数据，服务器可以限制从网络连接读取数据或关闭网络连接。建议将此作为避免拒绝服务攻击的一种方法。*

## 3.2 CONNACK – 连接回复

CONNACK 包是由服务器发送用来响应客户端发送的 CONNECT 包的MQTT包。<span class="vcMarked">服务器**必须**在发送除 AUTH 外的其他任何MQTT包之前使用带有响应码 0x00（成功）的 CONNACK 包回复客户端</span> <span class="vcReferred">[MQTT-3.2.0-1]</span>。<span class="vcMarked">服务器**必须**不在一次网络连接中发送超过一个 CONNACK 包</span> <span class="vcReferred">[MQTT-3.2.0-2]</span>。

如果客户端没有在合理的时间内收到来自服务器的 CONNACK 包，客户端**应该**断开网络连接。一个“合理”的时间取决于应用程序的类型和通信基础设施。

### 3.2.1 CONNACK固定头

固定头格式参考 图3-7。

*图3-7 CONNACK包固定头*

<table>
  <thead>
    <tr><td>Bit</td><td>7</td><td>6</td><td>5</td><td>4</td><td>3</td><td>2</td><td>1</td><td>0</td></tr>
  </thead>
  <tbody>
    <tr><td>byte 1</td><td colspan="4">MQTT包类型（2）</td><td colspan="4">保留</td></tr>
    <tr><td></td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>1</td><td>0</td></tr>
    <tr><td>byte 2</td><td colspan="8">剩余长度</td></tr>
  </tbody>
</table>

**剩余长度字段**

这是使用可变整数编码的值，代表可变头的长度。

### 3.2.2 CONNACK可变头

CONNACK 包中的可变头按序包括了下列字段：连接回复标识，连接原因码，属性集。属性集的编码方式参考 [2.2.2](#2-2-2-属性集)。

#### 3.2.2.1 连接回复标识

<span class="vcMarked">Byte 1 是 “连接回复标识”。Bits 7-1 是保留字段，**必须**被置为0</span> <span class="vcReferred">[MQTT-3.2.2-1]</span>。

Bit 0 是会话展示标识。

##### 3.2.2.1.1 会话展示

会话展示标识位于连接回复标识的 bit 0。

会话展示标识向客户端通知服务器是否在使用一个与客户端有相同客户端ID的连接的会话状态。这允许客户端和服务器对会话状态是否存在有一致的观点。

<span class="vcMarked">如果服务器接收连接的全新开始标识被置为 1，服务器**必须**在带有 0x00（成功）的原因码的 CONNACK 包中将会话展示置为 0</span> <span class="vcReferred">[MQTT-3.2.2-2]</span>。

<span class="vcMarked">如果服务器接收到的连接中全新开始位被置为 0，且服务器持有对此客户端ID的会话状态，服务器**必须**在 CONNACK 包中将会话展示标识置为 1，其他情况下，服务器都**必须**在 CONNACK 包中将会话展示标识置为 0。这两种情况下服务器都**必须**在 CONNACK 中使用原因码 0x00（成功）</span> <span class="vcReferred">[MQTT-3.2.2-3]</span>。

如果客户端从服务器接收到的会话展示的值不符合预期，客户端按照如下步骤继续：

- <span class="vcMarked">如果客户端不持有会话状态，且接收到的会话展示值为1，客户端**必须**关闭网络连接</span> <span class="vcReferred">[MQTT-3.2.2-4]</span>。如果客户端想要使用新的会话重启，客户端可以将全新开始标识置为 1 然后重新连接。
- <span class="vcMarked">如果客户端持有会话状态且收到的会话展示值为 0，如果客户端继续使用此网络连接，客户端**必须**丢弃会话状态</span> <span class="vcReferred">[MQTT-3.2.2-5]</span>。

<span class="vcMarked">如果服务器使用非 0 原因码的 CONNACK 包，服务器**必须**将会话展示的值置为 0</span> <span class="vcReferred">[MQTT-3.2.2-6]</span>。

#### 3.2.2.2 连接原因码

### 3.3.1 PUBLISH 固定头

# 4 操作行为

## 4.1 会话状态

### 4.1.1 存储会话状态

## 4.2 网络连接

## 4.7 主题名和主题过滤器

## 4.9 流量控制

## 4.10 请求 / 响应

## 4.12 增强认证

## 4.13 错误处理

# 5 安全性（非规范性）

### 5.4.9 处理禁止的Unicode码段

# 6 使用WebSocket作为传输层

# 7 一致性

# 附录 C. MQTT v5.0 新特性汇总（非规范性）