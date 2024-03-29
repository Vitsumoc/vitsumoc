---
title: "[翻译]Effective Go"
date: 2024-03-25 15:22:03
categories:
- Go
tags:
- Go
- 翻译
---

> 原文 [Effective Go](https://go.dev/doc/effective_go)

<!-- more -->

目录

- [简介](#简介)
  - [示例](#示例)
- [格式](#格式)
- [注释](#注释)
- [命名](#命名)
  - [包命名](#包命名)
  - [Getters](#Getters)
  - [接口命名](#接口命名)
  - [驼峰命名](#驼峰命名)
- [分号](#分号)
- [控制结构](#控制结构)
  - [If](#If)
  - [重声明与重分配](#重声明与重分配)
  - [For](#For)
  - [Switch](#Switch)
  - [Type switch](#Type-switch)
- [函数](#函数)
  - [多返回值](#多返回值)
  - [命名结果参数](#命名结果参数)
  - [Defer](#Defer)
- [数据](#数据)
  - [通过new分配](#通过new分配)
  - [构造器与复合字面量](#构造器与复合字面量)
  - [通过make分配](#通过make分配)
  - [数组](#数组)
  - [切片](#切片)
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
- [The blank identifier](#占位1)
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

## Getters

Go 没有提供对 getters 和 setters 的自动支持。但您可以自己添加 getters 和 setters，而且这也常常是很适合的，但是在 getter 的名称前加上 Get 即不必须，也不符合 Go 的习惯。假设你有一个名为 owner 的字段（小写，非导出），那么他的 getter 方法应该被命名为 Owner（大写，导出），而不是 GetOwner。使用大写名称来作为导出标志可以方便的区分字段和方法。如果需要的话，他的 setter 函数应该被命名为 SetOwner。这些名称在实践中具有很好的阅读性：

```go
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```

## 接口命名

按照约定，只有一个方法的接口会使用方法名加 -er 后缀或类似的修改，构成一个表示操作者的名词：Reader，Writer，Formatter，CloseNotifier 等等。

许多常用方法名已经约定俗成，遵循这些既有命名并保持其含义是高效的。Read，Write，Close，Flush，String 等都拥有标准的方法签名和含义。为了避免混淆，不要给你的方法起这些名称，除非它们的方法签名和含义完全相同。相反地，如果你的实现具有和现有类型完全相同的方法签名和含义，那么就应该使用相同的名称和方法签名。例如，将你的字符串转换方法命名为 String，而不是 ToString。

## 驼峰命名

最后，Go 中处理多个单词的名称时使用大驼峰命名或小驼峰命名，而非下划线连接。

# 分号

和 C 相同，Go 的语法使用分号来终止语句，但与 C 不同的是，这些分号不会出现在源码中。取而代之的是，词法分析器会使用一个简单的规则在扫描时自动插入分号，因此输入的文本基本上无需含有分号。

规则是这样的。如果在换行符之前的最后一个词是类型标志（比如 int 和 float64），基本字面量（比如一个数字或字符串），或是以下之一：

```text
break continue fallthrough return ++ -- ) }
```

词法分析器会在这些词后插入一个分号。可以这样总结“如果一个换行符前的词可以使用分号结尾，则插入一个分号”。

右大括号前的分号也可以省略，因此这样的语句

```go
 go func() { for { dst <- <-src } }()
```

无需分号。地道的 Go 程序只会在类似于 for 循环处使用分号，用来区分初始化语句、约束条件和循环动作。如果您必须要将多条语句写入同一行中，那么他们之间也需要分号。

分号插入规则带来的后果之一是你不能将左大括号放置在控制语句（if，for，switch，select）的下一行。如果您这样做了，大括号前会被插入一个分号，这会带来一些不良影响。应该这样写

```go
if i < f() {
    g()
}
```

而不是这样写

```go
if i < f()  // wrong!
{           // wrong!
    g()
}
```

# 控制结构

Go 中的控制结构与 C 息息相关，但在一些重要的方面又有所不同。没有 do 或 while 循环，只有一个更通用的 for；switch 更加灵活；if 和 switch 像 for 一样可以设置初始化语句；break 和 continue 语句采用可选标签来标识需要中断或继续的内容；有一些新的控制结构例如type switch and a multiway communications multiplexer, select。语法也略有不同：没有小括号，并且主体内容必须用大括号包裹。

## If

Go 中简单的 If 看起来是这样：

```go
if x > 0 {
    return y
}
```

强制要求的大括号鼓励使用多行书写简单的 if 语句。无论如何，这都是一种好的代码风格，特别是当主体内容包含 return 或 break 之类的控制语句的时候。

由于 if 和 switch 可以接收一个初始化语句，因此通常会看到他被用来设置局部变量。

```go
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

在 Go 库中，你会发现当 if 语句不流入下一个语句时——即内容主题使用 break，continue，goto 或 return 结尾——不必要的 else 会被省略。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
```

这是一个常见情况的示例，其中的代码必须考虑一系列的错误情况。代码的可读性很好，成功的步骤按序向下执行，错误总是在他出现的地方被处理。由于错误情况都使用 return 语句结束流程，因此不需要使用 else 语句。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```

## 重声明与重分配

旁白：上一节的例子展示了如何使用 := 进行短声明，他通过调用 os.Open 函数进行声明

```go
f, err := os.Open(name)
```

这行代码声明了两个变量，f 和 err。几行后，又调用了 f.Stat

```go
d, err := f.Stat()
```

这看起来是声明了 d 和 err。但请注意，这两行代码都出现了 err。这种重复是合法的：err 只被第一行代码声明，在第二行代码中只是被*重分配（re-assigned）*。这意味着调用 f.Stat 使用的是之前声明的 err，此处仅仅是给他赋予新的值。

在一个 := 赋值中，已经被声明的变量 v 仍然可以被重分配，只要：

- 本次声明和已经存在的对 v 的声明在同一个代码空间（如果 v 是在外部空间被声明的，那么本次声明会创造一个新的变量§），
- 初始化中响应的值可以被分配给 v，而且
- 本次声明中至少包括一个全新被声明的变量。

这样的非常规设计是纯粹的实用主义，是的使用单个 err 变得很容易，例如，在一长串的 if-else 中。你会经常看到这样的用法。

§ 这里值得注意的是，在 Go 中，函数的参数和返回值的空间与函数体相同，即使它们在词法上出现在函数体的大括号之外。

## For

Go 中的 for 循环和 C 中的 for 循环相似但又不同。他统一了 for 循环和 while 循环，并且没有 do-while 循环。总共有三种形式，其中只有一种带有分号。

```go
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }
```

短声明方式让声明循环中使用的索引变得容易。

```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
```

如果您在循环数组、切片、字符串或 map，又或是在读取 channel，range 关键字可以帮助你管理循环。

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

如果您只需要 range 中的第一个参数（key 或者 index），那么丢掉第二个：

```go
for key := range m {
    if key.expired() {
        delete(m, key)
    }
}
```

如果你只需要 range 中的第二个参数（值），使用空标识，也就是下划线，从而丢弃第一个参数：

```go
sum := 0
for _, value := range array {
    sum += value
}
```

空标识有很多种用法，就像[后续章节](#占位1)中描述的这样。

对于字符串，range 为您做了更多的事情，通过解析 UTF-8 来分解各个 Unicode 码。错误的编码消耗一个 byte 并使用 rune U+FFFD 代替（rune 是 Go 中称呼和使用单个 Unicode 码点的术语。参考[language specification](https://go.dev/ref/spec)了解更多）。对于下面的循环

```go
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```

输出为

``` text
character U+65E5 '日' starts at byte position 0
character U+672C '本' starts at byte position 3
character U+FFFD '�' starts at byte position 6
character U+8A9E '語' starts at byte position 7
```

最后，Go 没有逗号运算符， ++ 和 -- 是语句而不是表达式。因此如果你在 for 中运行多个变量，你应该使用多重赋值（尽管这排除了 ++ 和 --）。

```go
// Reverse a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
```

## Switch

Go 中的 switch 比 C 中的更通用。表达式不需要是常量，甚至不需要是整数，cases 从上向下匹配，直到寻找到匹配项，如果 switch 没有表达式，则他匹配到 true。因此，习惯上可以将 if-else-if-else 链编写为 switch。

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```

Go 语言的 switch 不会自动执行下一个分支，但是可以使用逗号分隔的列表来组合判断条件。

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

尽管他们在 Go 中不像在其他一些类似 C 的语言中那么常见，但 break 语句可以用来中止 switch。但有时，我们需要打破外围的循环，而非 switch，在 Go 中可以通过在循环上放置 label 并 "breaking" 该标签来完成。下面的例子演示了这两种用途。

```go
Loop:
    for n := 0; n < len(src); n += size {
        switch {
        case src[n] < sizeOne:
            if validateOnly {
                break
            }
            size = 1
            update(src[n])

        case src[n] < sizeTwo:
            if n+1 >= len(src) {
                err = errShortInput
                break Loop
            }
            if validateOnly {
                break
            }
            size = 2
            update(src[n] + src[n+1]<<shift)
        }
    }
```

当然， continue 语句也可以接受标签，但他仅适用于循环。

为了完成这个章节，这里有一个使用两个 switch 语句的字节切片比较函数。

```go
// Compare returns an integer comparing the two byte slices,
// lexicographically.
// The result will be 0 if a == b, -1 if a < b, and +1 if a > b
func Compare(a, b []byte) int {
    for i := 0; i < len(a) && i < len(b); i++ {
        switch {
        case a[i] > b[i]:
            return 1
        case a[i] < b[i]:
            return -1
        }
    }
    switch {
    case len(a) > len(b):
        return 1
    case len(a) < len(b):
        return -1
    }
    return 0
}
```

## Type switch

switch 也可以用来发现接口变量的动态类型。这种 type switch 使用类型断言的语法，并将关键字 type 放在括号内。如果 switch 在表达式中声明了变量，那么变量会在每个匹配项中获得相应的类型。在每个 case 项中使用相同的名称也是惯用的做法，实际上是在每个 case 中声明了名称相同但类型不同的变量。

```go
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t)             // t has type bool
case int:
    fmt.Printf("integer %d\n", t)             // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
```

# 函数

## 多返回值

Go 中的一个特殊特性是函数和方法可以返回多个值。这种形式可以用于改进 C 程序中的几个笨拙的惯用方法：例如通过返回 -1 来表示 EOF 或是修改一个使用指针传入的参数。

在 C 语言中，write 错误通过一个负数来表示，而错误代码隐藏在 volatile 变量中。在 Go 中， Write 可以同时返回计数和错误：“你写入了一些字节，但没有完全写入，因为你的磁盘满了”。在 os 包中的 Write 方法的方法签名是：

```go
func (file *File) Write(b []byte) (n int, err error)
```

正如文档所说，当 n != len(b) 时，函数返回了已经写入的字节数和一个非 nil 的 error。这是一个通用的风格，查看关于错误处理的章节了解更多例子。

一个类似的可以用来替代传递指针作为参数的方法是，通过返回一个值来模拟引用参数。这里是一个简单的函数，用来从字节切片的某个定位开始抓取数字，并返回数字和数字后的定位。

```go
func nextInt(b []byte, i int) (int, int) {
    for ; i < len(b) && !isDigit(b[i]); i++ {
    }
    x := 0
    for ; i < len(b) && isDigit(b[i]); i++ {
        x = x*10 + int(b[i]) - '0'
    }
    return x, i
}
```

您可以通过这样的方式使用，扫描切片 b 中的数字：

```go
    for i := 0; i < len(b); {
        x, i = nextInt(b, i)
        fmt.Println(x)
    }
```

## 命名结果参数

在 Go 函数中返回的结果“参数”可以被命名并像其他常规变量一样使用，就像输入参数一样。当被命名时，他们会在函数开始时被初始化为其对应类型的0值；如果函数执行不带参数的返回，那么这些结果参数的当前值会被用作返回值。

命名不是强制性的，但是他们会让代码更加简短和清晰：他们是文档的一种。如果我们将 nextInt 函数的返回结果命名，那么返回的 int 的含义将变得显而易见。

```go
func nextInt(b []byte, pos int) (value, nextPos int) {
```

因为命名的返回值已经和不带参数的 return 绑定在了一起，他们可以用来简化和澄清代码。这里是使用命名结果参数的 io.ReadFull：

```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```

## Defer

Go 中的 defer 语句可以在当前执行的函数返回前执行一次函数调用（ *deferred* 函数）。这是一个不寻常但有效的方案，可以用来处理类似与在任何函数执行路径下的资源关闭问题。典型的例子是解锁一个 mutex 或关闭一个文件。

```go
// Contents returns the file's contents as a string.
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```

Deferring 例如对 Close 函数的调用有两个好处。其一， 他确保了你不会忘记关闭这个文件，尤其是你之后再为函数添加返回路径时。其二，他意
味着 close 的位置与 open 在一起，这比将他放在函数的末尾要清晰的多。

deferred 函数的参数（或是 deferred 方法的接收者）是在 *defer* 执行时计算的，而非是在函数被调用时计算。除了无需担心在函数执行过程中的变量值改变外，这也意味着一个 defer 语句可以创建多个延迟执行的函数。这是一个愚蠢的例子。

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

Deferred 函数按照后进先出的顺序执行，因此这段代码会在函数返回时打印 4 3 2 1 0。一个更合理的例子是通过程序跟踪函数调用的简单方法。我们可以写一些类似这样的简单跟踪例程：

```go
func trace(s string)   { fmt.Println("entering:", s) }
func untrace(s string) { fmt.Println("leaving:", s) }

// Use them like this:
func a() {
    trace("a")
    defer untrace("a")
    // do something....
}
```

我们可以利用 deferred 函数是在 defer 运行时计算参数这一事实来做的更好。tracing 例程可以用来设置 untracing 例程的参数。下面是例子：

```go
func trace(s string) string {
    fmt.Println("entering:", s)
    return s
}

func un(s string) {
    fmt.Println("leaving:", s)
}

func a() {
    defer un(trace("a"))
    fmt.Println("in a")
}

func b() {
    defer un(trace("b"))
    fmt.Println("in b")
    a()
}

func main() {
    b()
}
```

输出

```text
entering: b
in b
entering: a
in a
leaving: a
leaving: b
```

对于在其他编程语言中习惯了块级资源管理的程序员来说，defer 也许看起来有些特殊，但是他最有趣也是最强大的的应用恰恰来自于他是基于函数而非基于块的事实。在 panic 与 recover 章节我们会看到这种可能性的例子。

# 数据

## 通过new分配

Go 中有两个分配关键字，内置函数 new 和 make。他们做不同的事情而且用于不同的类型，这可能会令人困惑，但是规则实际上很简单。让我们先从 new 开始。这是一个内建的用于分配内存的函数，但与一些其他编程语言中的同名函数不同，他并不会初始化内存，而只是将内存值置零。也就是说，new(T) 分配了一块零值的内存用来放置类型 T 并返回他的指针，一个类型为 *T 的值。在 Go 术语中，他返回了一个指向新分配的零值 T 的
指针。

由于 new 返回的内存值总是 0，因此在设计您的数据结构时，将 0 值作为一个合理的初始化值就非常有帮助。这意味着数据结构的使用者可以通过 new 来创建他并立刻使用。例如 bytes.Buffer 的文档表示“零值的 Buffer 是一个可以被使用的空 buffer”。类似的，sync.Mutex 也没有显式的构造函数或是初始化方法。取而代之的是，零值的 sync.Mutex 被定义为一个未锁定的 mutex。

零值有效的设计方式是可以传递的，考虑下述类型声明。

```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```

类型为 SyncedBuffer 的值也可以在被分配或声明后立即使用。在下面的片段里，p 和 v 都可以在无需更多参数的情况下正确使用。

```go
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

## 构造器与复合字面量(composite literals)

有些情况下无法通过零值初始化，这时就需要构造器，例如这个 os 包中的例子。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}
```

这里有很多的赋值操作。我们可以简单的使用复合字面量，这是一种在每次执行时创建一个新实例的表达式。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

请注意，与 C 不同，返回局部变量的地址是完全可行的；在函数返回后与变量关联的存储依然存在。实际上，每次获取复合字面量地址时都会分配一个新的实例，因此我们可以合并最后两行。

```go
    return &File{fd, name, nil, 0}
```

复合字面量中的字段必须按序且全部存在。然而，通过使用 field:value 对标记元素，初始值设定可以以任何顺序出现，缺失的字段保留位各自的零值。因此我们可以

```go
    return &File{fd: fd, name: name}
```

作为一个有限的情况，如果复合字面量没有包括任何字段，他会创建当前类型的零值对象。表达式 new(File) 和 &File{} 是等效的。

复合字面量也可以用来创建数组、切片和 maps，其中的字段标签被视为索引或 map 中的键。在这些示例中，只要 Enone、Eio、Einval 的值不同，无论他们的值如何，初始化都会生效。

```go
a := [...]string   {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
s := []string      {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
m := map[int]string{Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
```

## 通过make分配

回到内存分配的话题。内置函数 make(T, args) 提供了一个与 new(T) 不同的用途。他只用来创建切片、maps 和 channels，而且他返回一个已经被初始化的（非零值）的 T 的值（而非 *T）。区别的原因是这三种类型在底层都是对必须被初始化的某种数据结构的引用。例如切片，由三个元素组成，指向数据的指针（数据是内置的数组）、长度和容量，在这些元素被初始化前，切片的值是 nil。对于切片、maps 和 channels，make 初始化了他们内部的数据结构而且提供了可用的值。对于，

```go
make([]int, 10, 100)
```

分配了一个长度为 100 int 的数组，随后创建切片结构，长度为 10，容量为 100，指向数组最初的 10 个元素。（在创建切片时，可以不手动指定容量；参考切片章节了解更多信息）。相反，new([]int) 返回一个新分配的，值为零的切片结构的指针，也就是一个指向 nil 切片值的指针。

这些例子阐明了 new 和 make 的区别。

```go
var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints

// Unnecessarily complex:
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// Idiomatic:
v := make([]int, 100)
```

请记住，make 用于 maps，切片和 channels，并不返回指针。如果想要显式的获得指针，需要使用 new 或是显式的获取变量的地址。

## 数组

数组在进行详细的内存规划时很有用，而且有时候可以用来避免分配动作，但是大部分时候他们还是用于作为切片的一部分使用，也就是下一个章节的主题。为了给下一个主题打下基础，这里简单介绍一下数组。

这里有一些 Go 中数组和 C 中数组的主要区别。在 Go 中，

- 数组是值，将一个数组赋值给另一个会复制所有的元素。
- 特别是，将数组传递给函数，他会收到数组的副本，而不是数组的指针。
- 数组的尺寸使其类型的一部分。[10]int 和 [20]int 是不同的类型。

使用值传递可能会有用，但是开销也很大；如果你想要和 C 相似的表现方式和效率，你可以传递一个数组的指针。

```go
func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a {
        sum += v
    }
    return
}

array := [...]float64{7.0, 8.5, 9.1}
x := Sum(&array)  // Note the explicit address-of operator
```

但是这也不是 Go 中的惯用方式，Go 程序员使用切片。

## 切片

切片封装了数组，提供了一个更加通用、强大、方便的处理序列数据的接口。除了例如变换矩阵这种有显式的维度的情况外，在 Go 中大部分关于顺序数据的编程都是通过切片而非数组完成的。

切片持有了对其下层数组的引用，如果你将一个切片赋值给另一个，他们将会引用同一个数组。如果函数使用切片作为参数，则调用者可以看到函数对切片中元素的修改，类似于传递了下层数组的指针。因此，Read 函数可以接收切片作为参数，而非指针和计数；切片中的长度设置了要读取数据内容的上线。这里是 os 包中 File 类型的 Read 方法的方法签名：

```go
func (f *File) Read(buf []byte) (n int, err error)
```

该方法返回读取的字节数和可能的错误值。如果要读取一个 buffer 的前32个字节，可以对 buffer 进行切片（此处为动词）。

```go
    n, err := f.Read(buf[0:32])
```

Such slicing is common and efficient. In fact, leaving efficiency aside for the moment, the following snippet would also read the first 32 bytes of the buffer.

```go
    var n int
    var err error
    for i := 0; i < 32; i++ {
        nbytes, e := f.Read(buf[i:i+1])  // Read one byte.
        n += nbytes
        if nbytes == 0 || e != nil {
            err = e
            break
        }
    }
```

The length of a slice may be changed as long as it still fits within the limits of the underlying array; just assign it to a slice of itself. The capacity of a slice, accessible by the built-in function cap, reports the maximum length the slice may assume. Here is a function to append data to a slice. If the data exceeds the capacity, the slice is reallocated. The resulting slice is returned. The function uses the fact that len and cap are legal when applied to the nil slice, and return 0.

```go
func Append(slice, data []byte) []byte {
    l := len(slice)
    if l + len(data) > cap(slice) {  // reallocate
        // Allocate double what's needed, for future growth.
        newSlice := make([]byte, (l+len(data))*2)
        // The copy function is predeclared and works for any slice type.
        copy(newSlice, slice)
        slice = newSlice
    }
    slice = slice[0:l+len(data)]
    copy(slice[l:], data)
    return slice
}
```

We must return the slice afterwards because, although Append can modify the elements of slice, the slice itself (the run-time data structure holding the pointer, length, and capacity) is passed by value.

The idea of appending to a slice is so useful it's captured by the append built-in function. To understand that function's design, though, we need a little more information, so we'll return to it later.