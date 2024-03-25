---
title: "[翻译]Effective Go"
date: 2024-03-25 15:22:03
categories:
- Go
tags:
- Go
- 翻译
---

目录

- [简介](#简介)
  - [示例](#示例)
- [格式](#格式)
- [注释](#注释)
- [命名](#命名)
  - [包命名](#包命名)
  - [Getters]
  - [Interface names]
  - [MixedCaps]
- [Semicolons]
- [Control structures]
  - [If]
  - [Redeclaration and reassignment]
  - [For]
  - [Switch]
  - [Type switch]
- [Functions]
  - [Multiple return values]
  - [Named result parameters]
  - [Defer]
- [Data]
  - [Allocation with new]
  - [Constructors and composite literals]
  - [Allocation with make]
  - [Arrays]
  - [Slices]
  - [Two-dimensional slices]
  - [Maps]
  - [Printing]
  - [Append]
- [Initialization]
  - [Constants]
  - [Variables]
  - [The init function]
- [Methods]
  - [Pointers vs. Values]
- [Interfaces and other types]
  - [Interfaces]
  - [Conversions]
  - [Interface conversions and type assertions]
  - [Generality]
  - [Interfaces and methods]
- [The blank identifier]
  - [The blank identifier in multiple assignment]
  - [Unused imports and variables]
  - [Import for side effect]
  - [Interface checks]
- [Embedding]
- [Concurrency]
  - [Share by communicating]
  - [Goroutines]
  - [Channels]
  - [Channels of channels]
  - [Parallelization]
  - [A leaky buffer]
- [Errors]
  - [Panic]
  - [Recover]
- [A web server]

<!-- more -->

<head>
  <style>
    .indentation {
      margin-left: 2rem;
    }
  </style>
</head>

# 简介

Go 是一门新语言。虽然他借鉴了现有编程语言的思想，但他也具有独特的特性，使得有效的 Go 程序与其他语言编写的程序有性质上的不同。直接将 C++ 或 Java 程序改编为 Go 程序可能无法得到一个让人满意的结果——Java 程序是使用 Java 编写的，而不是 Go。另一方面，从 Go 的角度思考问题可能会产生一个成功但完全不同的程序。换句话说，要写好Go，重要的是要理解它的属性和习惯用法。了解 Go 编程的既定约定也很重要，例如命名、格式、程序构造等，这样您编写的程序将易于其他 Go 程序员理解。

这份文档提供了编写清晰、地道的 Go 代码的建议。他补充了 [language specification](https://go.dev/ref/spec)、[the Tour of Go](https://go.dev/tour/)以及 [How to Write Go Code](https://go.dev/doc/code.html) 这几份资料，建议您在阅读本指南之前先阅读这些资料。

2022 年 1 月注：这份指南最初发布于 2009 年，自那以来更新并不频繁。虽然他仍然是理解如何使用 Go 语言本身的良好指南（因为语言本身很稳定），但他几乎没有涉及到 Go 生态系统自发布以来的重大变化，例如构建系统、测试、模块化和多态性。由于近年来变化太多，官方也不会再更新此文档。目前已经有很多文档、博客和书籍详细描述了现代 Go 的用法，足以满足学习需求。因此，虽然这份「Effective Go」指南仍然可用，但读者需要理解它并不是全面的指南。详情请参阅 [issue 28782](https://go.dev/issue/28782)。

## 示例

[Go package sources](https://go.dev/src/)不仅是核心库，还包含了如何使用该语言的示例。此外，许多标准库包都包含可直接在 [go.dev](https://go.dev/) 网站上运行的独立可执行示例（例如[本例](https://go.dev/pkg/strings/#example-Map)，如果需要，可以点击 Example 按钮打开它）。如果您对如何解决问题或如何实现某项功能有疑问，那么标准库中的文档、代码和示例可以提供答案、思路和背景信息。

# 格式

格式问题是争议最大但影响最小的问题。人们可以适应不同的格式风格，但如果没有必要的话，最好保持统一，这样大家就可以少花时间在格式上争论。 难点在于如何在没有冗长强制性风格指南的情况下实现这种「乌托邦」式的统一格式。

对于 Go 代码格式，我们采用了一种不同寻常的方法，让机器来处理大多数格式化问题。gofmt 程序（也可以称为 go fmt，它在包级别而不是源文件级别工作）读取一个 Go 程序，并按照标准的缩进和垂直对齐样式输出源代码，同时保留并根据需要重新格式化注释。如果您想了解如何处理一些新的布局情况，请运行 gofmt； 如果答案看起来不对，请重新排列您的程序（或提交有关 gofmt 的 bug 报告），不要试图绕过它。

例如，不必浪费时间去对齐结构体中的注释，Gofmt 会协助你处理，对于如下声明：

```go
type T struct {
    name string // name of the object
    value int // its value
}
```

gofmt 会将列对其：

```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

所有标准库中的 Go 代码都使用 gofmt 进行了格式化。

简要的介绍一下格式化的细节：

缩进

<div class="indentation">
我们使用制表符 (tab) 进行缩进，gofmt 工具默认也会采用这种方式。只有在绝对必要的情况下才使用空格。
</div>

<br />

行长

<div class="indentation">
Go 没有代码行长限制。不用担心代码会超出打孔卡的限制。如果一行代码看起来太长，可以将其换行并使用额外的制表符缩进。
</div>

<br />

括号

<div class="indentation">
与 C 和 Java 语言相比，Go 需要更少的括号：控制结构 (if、for、switch) 的语法本身不需要括号。此外，Go 的运算符优先级层次更短更清晰。因此

```go
x<<8 + y<<16
```

通过间隔暗示了执行顺序，这一点和其他编程语言不同。
</div>

# 注释

Go 提供了 C 风格的 /* */ 块状注释和 C++ 风格的 // 行注释。通常情况下会使用行注释，块状注释往往是在包注释中使用，但也可以用在表达式中或禁用大块的代码。

位于顶层声明之前的注释，如果没有额外的空行隔开，就会被视为该声明的文档。这些“文档注释”是 Go 程序包或命令的主要文档形式。有关文档注释的更多信息，请参阅“[Go Doc Comments](https://go.dev/doc/comment)”。

# 命名

在 Go 中，命名和其他语言一样重要。他们甚至具有语义上的影响：一个名称对于包外部的可见性取决于他的首字母是否大写。因此，值得花一点时间讨论 Go 程序中的命名约定。

## 包命名

当一个包被引入时，包名称就成为了其内容的入口，当做了如下引入之后

```go
import "bytes"
```

引入该包的程序可以使用 bytes.Buffer。所有人都使用相同的名称来引用包的内容有很多好处，这也意味着包的命名必须是优秀的：短，简洁，意义明确。按照约定，包使用小写单个单词命名，不应出现下划线或驼峰命名。宁可过于简洁，因为每个使用您的包的人都需要输入这个名称。也无需担心包名的重复，包名只是引入时的默认名称，他不必在所有的源码中保持唯一，在极少数的包名重复的情况下，引入包可以选择一个本地使用的别名。无论如何，包名重复都是很罕见的，因为引入中的文件名决定了在使用哪个包。

另一个约定是包名是他源文件夹的基础名称，src/encoding/base64 中的包被 "encoding/base64" 引入，但使用名称为 base64，并非 encoding_base64 或 encodingBase64。

引入包的程序会使用包名来引用包中的内容，因此包导出的名称可以利用这一点来避免重复（不要使用 import . 来省略包名，他可以简单的运行一些必须在包外部运行的测试，但应该在其他场合下避免）。例如，在 bufio 包中的缓冲读取器被命名为 Reader，而不是 BufReader，因为使用者看到的将会是 bufio.Reader，这是一个清晰、简洁的名称。此外，由于引入的内容总是和引入的包名一起使用，因此 bufio.Reader 并不会和 io.Reader 冲突。相似的，创建 ring.Ring 新实例的函数——也就是 Go 中的构造函数——通常被命名为 NewRing，但是考虑到 Ring 是 ring 包中导出的唯一类型，而且又因为包名已经是 ring，所以这个函数只需被命名为 New，包的使用者看到的是 ring.New。利用包的结构可以帮助你选择合适的命名。

另一个简短的示例是 once.Do，once.Do(setup) 读起来非常友好，而且明显优于 once.DoOrWaitUntilDone(setup)。长的命名不一定能带来更丰富的含义。一个实用的文档注释常常比长的命名更有价值。