---
title: vmq-从零开始的MQTT客户端-3-fyne
date: 2024-02-15 13:42:26
categories:
- vmq
tags:
- vmq
- MQTT
- 网络编程
- golang
- fyne
---

探索如何使用 fyne 构建需要的图形化页面并不会是一个非常复杂的过程，但也需要一定的时间。按照习惯我会把[官方指引](https://docs.fyne.io/started/)通读一遍，并实验其中所有的示例。

<!-- more -->

# fyne仓库

指引中已经非常详细的介绍，我也不必将内容复述一遍，我将我的示例实现放到这个[仓库](https://github.com/vitsumoc/learn-fyne)中，这仅仅是一个记录，没有任何创造性的内容。

# 接下来

既然已经掌握了 fyne 的基本用法，接下来就到了实现 原型图/需求 的时候了，从最基础的连接管理开始，看上去是个不错的主意。