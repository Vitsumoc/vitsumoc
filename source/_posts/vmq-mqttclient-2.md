---
title: vmq-从零开始的MQTT客户端-2-框架
date: 2024-02-05 15:21:20
categories:
- vmq
tags:
- vmq
- MQTT
- 网络编程
- golang
---

按照既定思路搭建基本的软件框架：两个仓库，一个是 MQTT 客户端，另一个是图形化测试工具，图形化测试工具需要依赖我们的客户端。

<!-- more -->

两个仓库分别是 [vmq](https://github.com/vitsumoc/vmq) 和 [vmq-test-gui](https://github.com/vitsumoc/vmq-test-gui)

# 最初的vmq

让 vmq 提供一个可以被外部访问的函数：

```go test.go
func HelloVmq() string {
	fmt.Println("I'm vmq")
	return "I'm vmq"
}
```

现在只是一个 Hello World，但是以后他可以是一个创建连接的函数、创建客户端的函数、执行订阅的函数等等。总之原理是一样的，这是一个可以被 vmq-test-gui 调用的函数。

# 最初的gui

接下来是 vmq-test-gui 项目，既然是 gui，意味着需要一个图形化库来显示窗口，我没有这方面的经验，凭感觉选择了 [fyne](https://fyne.io/)。

参考 fyne 中的 Hello World 示例，只不过是把 vmq 塞进去：

```go test.go
func main() {
	a := app.New()
	w := a.NewWindow("Hello")

	hello := widget.NewLabel("Hello Fyne!")
	w.SetContent(container.NewVBox(
		hello,
		widget.NewButton("Hi!", func() {
			hello.SetText(vmq.HelloVmq())
		}),
	))

	w.ShowAndRun()
}
```

到这一步的结果符合我们的预期：

![](test.png)

# 接下来

至此为止的事情都非常简单，而接下来的事情会开始有一些挑战性。

我比较喜欢用需求来推动软件的功能，因为我觉得合理的软件设计是很难凭空产生的，但是合理的需求是易于理解和提出的，一旦有了合理的需求，那么能够实现需求的设计就可以视为合理的软件设计。

对于 vmq 也是一样，vmq-test-gui 就是 vmq 的需求方，我们的开发总是会从 gui 开始，先做出需要功能的界面，再在 vmq 中实现功能。

图形化界面开发会有一些难度，fyne 的学习应该也会有一些成本，但这些都会是值得的。