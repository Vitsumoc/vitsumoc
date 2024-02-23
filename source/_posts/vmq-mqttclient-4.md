---
title: vmq-从零开始的MQTT客户端-4-连接管理原型
date: 2024-02-21 15:51:06
categories:
- vmq
tags:
- vmq
- MQTT
- 网络编程
- golang
- fyne
---

所有的 MQTT 操作都是在一个连接的基础下进行的，所以创建连接显然是一切 MQTT 开发的基础。按照惯例，最开始我们用最简单的方式实现一切，比如：

<!-- more -->

- 使用 TCP 作为传输层
- 使用最简单的 CONNECT 包创建连接，只接收服务器地址和端口作为参数，其他参数均使用默认值
- 不实现 DISCONNECT 包，关闭连接时直接关闭 TCP
- 创建连接，查看连接，断开连接，删除连接，除此之外不做任何事

当然，在这一节我们只会做出页面原型，实际的数据实现工作还有数据和页面打通的工作，应该会在后续两节中完成。

# 页面

按照这些需求，做出的页面内容非常简单：

![](main.png)

![](dialog.png)

# 实现

本次的代码改动全部在 `vmq-tool` 项目中，本以为只是对 fyne 框架的简单应用，后来发现还是有一些架构设计的难度。

`vmq-tool` 里大概有四部分的内容：

- `vmq` 中实现的 MQTT 数据：例如连接、会话、应用消息
- 组织 MQTT 数据的数据：例如连接池，连接的配置，各种业务函数
- UI组件：例如页面，弹窗，标签
- UI中呈现的数据：例如标签的内容，弹窗的内容

fyne 框架例子中的各种界面，组件都是零散实现的，UI 内容和数据内容也常常是随手放在一起的，很容易想象的是，如果我们使用这种方式去实现我们的 `vmq-tool`，越到后期我们就会越发难以维护。UI 和数据都散落在各处，而且数据可能被各个地方需要，每个地方都写满了各种各样的更新函数、回调函数等等，每一个业务逻辑线都通过若干个随机的源码文件实现。

为了从一开始就避免这种情况的发生，我进行了一些简单的分层设计和总线设计，至少能够避免项目从一开始就陷入混乱的境地：

- 将 `vmq-tool` 分为 app 和 ui 两个模块，app 模块负责处理和 vmq 数据和相关的 MQTT 数据业务，ui 模块则专注于页面、弹窗等各种可见内容
- ui 模块分为四类文件，总线（ui.go）、页面、弹窗、组件
- 规定 app 模块可以向下调用 ui 中的事件，而 ui 模块只能通过回调影响 app 中的数据，ui 和 APP 的交互均使用 ui 中定义的数据结构
- 将所有事件、回调、弹窗等内容集中管理，形成四条总线，页面总线（所有ui页面的集合）、事件总线（app控制ui）、回调总线（ui控制app）、弹窗总线（所有弹窗的统一入口）

拆分后的 app.go 看起来十分清晰，因为还没有接入 `vmq` 的 MQTT 业务数据，所以需要做的事情只有启动 UI：

```go app.go
package app

import (
	fyneApp "fyne.io/fyne/v2/app"
	"github.com/vitsumoc/vmq-tool/ui"
)

func Run() {
	// 主窗口
	a := fyneApp.NewWithID("vitsumoc.github.vmqtool")
	// 初始化UI 并运行窗口
	ui.Init(a)
}

```

连接管理的页面逻辑也很简单，用一个自定义结构体（connectList）包装 fyne 使用的 UI 和其他我可能会用到的数据，通过 new 函数创建并由总线统一管理，这里我在文件名中用 page 前缀表示他是页面。

```go page_connect_list.go
// 连接管理列表
package ui

import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/container"
	"fyne.io/fyne/v2/widget"
)

type connectList struct {
	// 顶层布局
	content *fyne.Container
	// 交互部分
	btnAdd *widget.Button // 添加连接

	// 数据部分
	connList *widget.List    // 列表容器
	listData []*connectLabel // 数据内容
}

func newConnectList() *connectList {
	cl := &connectList{}

	cl.listData = []*connectLabel{
		NewConnectLabel("192.168.1.1:1883"),
		NewConnectLabel("192.168.1.1:1883"),
		NewConnectLabel("192.168.1.1:1883"),
	}
	cl.connList = widget.NewList(
		func() int {
			return len(cl.listData)
		},
		func() fyne.CanvasObject {
			return NewConnectLabel("192.168.1.1:1883")
		},
		func(i widget.ListItemID, o fyne.CanvasObject) {
			o.(*connectLabel).update()
		},
	)
	cl.btnAdd = widget.NewButton("创建连接", func() { diaBus("newConnect") })
	cl.content = container.NewBorder(cl.btnAdd, nil, nil, nil, cl.connList)

	return cl
}

```

弹窗的设计思路也相同，使用自定义结构体封装 ui，提供后面可能使用的各种数据或 ui 组件，然后将这些 ui 内容都通过总线加载，通过总线管理：

```go dia_new_connect.go
// 创建新连接
package ui

import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/container"
	"fyne.io/fyne/v2/dialog"
	"fyne.io/fyne/v2/layout"
	"fyne.io/fyne/v2/widget"
)

// 页面数据
type DATA_NewConnect struct {
	IPAddr string // ip地址
	Port   string // 端口号
}

// 页面UI
type newConnect struct {
	// 顶层布局
	content *fyne.Container
	// 弹层对象
	dia *dialog.CustomDialog
	// 组件
	labelIp   *widget.Label
	labelPort *widget.Label
	inputIp   *widget.Entry
	inputPort *widget.Entry
	btnAdd    *widget.Button
	btnCancel *widget.Button

	// 数据
	data DATA_NewConnect
}

// 创建页面方法
func newNewConnect() *newConnect {
	nc := &newConnect{}
	nc.labelIp = widget.NewLabel("IP地址")
	nc.labelPort = widget.NewLabel("端口")
	nc.inputIp = widget.NewEntry()
	nc.inputPort = widget.NewEntry()
	nc.btnAdd = widget.NewButton("添加", nil)
	nc.btnCancel = widget.NewButton("取消", func() { nc.dia.Hide() })

	// 内容对象
	nc.content = container.New(
		layout.NewFormLayout(),
		nc.labelIp, nc.inputIp,
		nc.labelPort, nc.inputPort,
		widget.NewLabel(""), container.NewHBox(nc.btnAdd, nc.btnCancel),
	)

	// 弹层对象
	nc.dia = dialog.NewCustomWithoutButtons("创建新连接", nc.content, mainWindow)

	return nc
}

// 清理数据 弹出连接弹窗
func (p *newConnect) pop() {
	// 清理数据
	p.data.IPAddr = ""
	p.data.Port = ""
	// 弹出弹窗
	p.dia.Show()
}

```

最后，是一个初始版的总线，现在他承载着初始化所有页面、转发所有上下行事件、控制所有弹窗等一系列复杂的工作，如果后续总线文件过于复杂，我们再考虑把这些职能拆分开来。起码现在这些内容放在一起，对于我们来说是比较容易管理的：

```go ui.go
// 存储并初始化所有页面, 为 app 提供管理入口
package ui

import (
	"fyne.io/fyne/v2"
	"fyne.io/fyne/v2/container"
	"fyne.io/fyne/v2/widget"
)

var mainWindow fyne.Window

// ui库结构
type suis struct {
	connectList *connectList
	newConnect  *newConnect
}

// ui库实例
var uis = suis{}

// UI初始化
func Init(a fyne.App) {
	// 主窗口初始化
	mainWindow = a.NewWindow("vmqtool")
	mainWindow.SetMaster()

	// 所有页面
	uis.connectList = newConnectList()
	uis.newConnect = newNewConnect()

	// 左右布局, 左边是连接列表, 右边是点击连接后的详情
	split := container.NewHSplit(uis.connectList.content, widget.NewLabel("right"))
	split.Offset = 0.2
	mainWindow.SetContent(split)

	mainWindow.Resize(fyne.NewSize(1280, 720))
	mainWindow.ShowAndRun()
}

// 弹窗总线 弹窗名 参数
func diaBus(name string, args ...interface{}) {
	if name == "newConnect" {
		uis.newConnect.pop()
	}
}

// 向UI下发事件总线 通过 数据 改变ui的 使用该总线直接下发

// UI向上回调总线 通过 ui 影响数据的 使用 app 注册的回调函数实现

```

通过上述规则的设置，现在的代码看起来更加有迹可循，其他业务的扩容也变得更简单。我并非 GUI 项目架构的专家，所以这些设计可能仍有很多可以改进之处，但是好在 `vmq` 系列是一个在实验中逐步推进的项目，以后我还有机会将他变得更好。

# 结构和源码

经过修改后的 `vmq-tool` 变成了这样的结构：

```text
vmq-tool/
 ├── app/
 │    └── app.go
 ├── ui/
 │    ├── comp_ui_connect_label.go
 │    ├── dia_new_connect.go
 │    ├── page_connect_list.go
 │    └── ui.go
 ├── vmqtool/
 │    └── vmqtool.go
```

本次的源码在 [这里](./vmq-mqttclient-4/vmq-tool.zip)