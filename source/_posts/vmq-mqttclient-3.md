---
title: vmq-从零开始的MQTT客户端-3-fyne
url: vmq-mqttclient-3
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

```go 示例列表
	firstApp() // 基础的程序、窗口
	demo() // fyne 官方示例
	appLoop() // 窗口和主循环
	updateContent() // 内容更新
	windows() // 窗口管理
	guiTest() // 图形化程序测试
    //07build.bat // 打包选项 构建源文件
	painter() // 手动绘制
	layout() // 布局能力
	shortcuts() // 窗口快捷键 组件快捷键
	preferences() // 首选项存取
	sysTray() // 系统托盘
	dataBinding() // 数据绑定
	drawing() // 图形绘制
	animation() // 动画
	layoutBox() // box 布局
	layoutGrid() // 网格布局
	layoutGridWarp() // 自适应网格布局
	layoutBorder() // 盒子布局
	layoutForm() // 表单布局
	layoutCenter() // 居中布局
	layoutMax() // 占满布局
	appTab() // 标签页
	fWidgets() // 各种组件
	fForm() // 表单
	fToolBar() // 工具栏
	fContainer() // 容器（列表 表格 树
	fDataBinding() // 数据绑定详细
	customLayout() // 自定义布局
	customWidget() // 自定义组件
	// 31 打包静态资源
	customTheme() // 自定义主题
	extendingWidgets() // 扩展现有组件
	fNumericalEntry() // 自定义数字输入组件
```

# 接下来

既然已经掌握了 fyne 的基本用法，接下来就到了实现 原型图/需求 的时候了，从最基础的连接管理开始，看上去是个不错的主意。