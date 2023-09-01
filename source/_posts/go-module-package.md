---
title: go module 与 package
date: 2023-03-24 15:23:51
tags: Golang
categories: Golang
description: 笔者在学习 golang 语言的时候，对其版本管理产生了诸多困惑，于是花费了几天时间搜集了很多资料，试图理解其背后的原理，现将搜集的资料整理成下文。
---

# 前言

笔者在学习 golang 语言的时候，对其版本管理产生了诸多困惑，于是花费了几天时间搜集了很多资料，试图理解其背后的原理，现将搜集的资料整理成下文。建议大家在理解这一部分的时候去看看 Go 团队的 leader Russ Cox 的解释：[Go & Versioning]([ https://research.swtch.com/vgo )，其中文翻译网址为：[note/go\_and\_versioning at master · vikyd/note · GitHub](https://github.com/vikyd/note/tree/master/go_and_versioning)

# 正文

软件是由代码组成的。为了复用代码，代码的组织出现了不同层次的抽象和实现，如 Module（模块），包（Package），Lib（库），Framwork（框架）等。

## package

Golang 使用包（package）这种语法元素来组织源码，所有语法可见性均定义在 package 这个级别，与 Java 、python 等语言相比，这算不上什么创新，但与 C 传统的 include 相比，则是显得“先进”了许多。

C 语言中所有未加 static 前缀的全局变量和函数都具有全局可见性；Java、C++则通过 public/private 关键字来定义可见性；在 Go 语言中，没有特别的关键字来声明一个方法、函数或者类型是否为公开的，Go 语言是以大小写方式进行区分的，如果一个类型的名字是以大写开头，那么其他包就可以访问；如果以小写开头，其他包就不能访问。

go 语言中一个文件夹即为一个包 (package)，也就是说一个文件夹下的同级源代码文件必须声明成一个包，这些同级源代码文件相当于一个文件，拆分成不同文件，一方面体现了 go 小而简洁的哲学，另一方面便于阅读和维护。允许文件夹名与包名不同，根据惯例，建议文件夹名与包名相同。注意：

- import 后面的是目录（即包的路径），一般为项目名 + 路径；
- 包名和目录名没有关系，但是包名最好等于目录名；
- 同一个目录下只能有一种包名；
- 一个包可以内嵌子包。

### main package

一个 go 模块要编译成可执行二进制文件，必须有包含 main 函数的 main 包。不像解释性语言，go 模块可以编译成一个不需要任何依赖就可以执行的二进制文件。

## go module

### 依赖管理

我们想复用已有的工作成果，将已有的工作成果加入到我们的项目中作为依赖存在着太多的不确定性：这个包的 API 会变化；这个包内部行为会变化；这个包的依赖会变化；这个包可能已经已经不存在或无法访问；包与包之间的不同依赖相互冲突等等。不仅如此，随着软件开发规模的逐步增大，涉及到的外部依赖越来越多，手动管理的所有依赖愈发不可能。所以我们需要依赖管理，我们需要有个工具或者规范来描述和定义包与包之间的依赖关系，并自动化的去处理、解析和满足这些依赖。

依赖管理试图解决的问题主要有两个：
其一是 `API 稳定性`，不会因为我们更新了一个小版本就要大规模的重写我们的代码。
其二是`可重现构建`，可重现构建在我们依赖管理的领域内可以理解为相同的源码最后能得到同样的二进制或链接库（当然真正的想实现可重现构建还需要一系列配套的工具和特定的参数）。

### 早期 GOPATH 

在 go1.12 之前，安装 golang 之后，需要配置两个环境变量 `$GOROOT` 和 `$GOPATH`。前者是 go 安装后的所在的路径，后者是开发中自己配置的，用于存放 go 源代码的地方。在 GOPATH 路径内，有三个文件夹，分别是：

- bin: go 编译后的可执行文件所在的文件夹
- pkg: 编译非 main 包的中间连接文件
- src: 存放项目的源代码，可以是你自己写的代码，也可以是你 go get 下载的包

\$GOPATH 即为我们的工作目录，所有的项目源代码都放在 \$GOPATH 路径下的 src 目录中。在这个模式下，使用 go install 时，生成的可执行文件会放在 `$GOPATH/bin` 下，如果你安装的是一个库，则会生成 .a 文件到 `$GOPATH/pkg` 下对应的平台目录中（由目标操作系统 `GOOS` 和目标架构 `GOARCH` 组合而成），生成 .a 为后缀的文件。

早期的这种模式源于 Google 的开发方式，Google 本身就有一个中央集中式中心用来管理软件包，使用，构建都从那里获取，因为外人无法访问 google 的中央，但是这种引用中央的开发模式被保留下来，因此就抽象成本地的 GOPATH。

使用这种模式会产生很多问题，最主要的是版本管理问题：

- 你无法在你的项目中，使用指定版本的包，因为不同版本的包的导入方法也都一样
- 其他人运行你的开发的程序时，无法保证他下载的包版本是你所期望的版本，当对方使用了其他版本，有可能导致程序无法正常运行
- 在本地，一个包只能保留一个版本，意味着你在本地开发的所有项目，都得用同一个版本的包，这几乎是不可能的。

### 早期解决方案

![go 依赖管理](/img/golang/go_dep_management.png)
Go 语言的依赖管理经历了漫长的迭代和演进，最终随着 [Go Modules](https://golang.org/ref/mod) 被官方采纳，形成大一统局面。

##### Go 依赖管理的演进历史

**阶段一：Makefile、goinstall、go get**
早在2009年11月，Go 最初版本的发布只包括：编译器、连接器和一些基本的库。此时 Go 程序需要自己运行 `6g` 和 `6l` 完成编译过程，因此官方发布的程序中也附加了一些 Makefile 的示例。

2010年2月，官方引入 goinstall（注意：这里的 goinstall 和 go 命令中的 go install 不是同一个概念）。这是一个零配置的命令，从代码仓库中直接下载包。goinstall 在当时引入了现在开发者已经非常熟悉的包导入规则，后面逐步形成明朗的生态系统。goinstall 终结了 Makefile 中五花八门实现造成的复杂性，这种化简对于形成统一工具链也具有极大正面作用。

2011年12月，go get 代替 goinstall 进入 go 命令，并成为 Go 官方支持的工具链。总体而言，go get 相比于 C/C++ 的依赖管理来说有革命性进步：

1.  允许 Go 的开发者方便地共享代码，和依赖他人的包进行构建。
2.  将构建系统的细节隔离在 go 命令中。

但 go get 缺少版本管理功能，当 go get 请求一个包的时候，它总是拉取最新的代码，并将拉取动作交给 Git、Mercurial 等版本管理系统去做。go get 对于版本管理的缺少至少带来两个致命问题。

**阶段二：语义版本和 API 稳定性**
go get 的第一个致命问题：由于没有版本的概念，在包更新时候它不知道该以什么方式通知用户（兼容性的还是非兼容性的）。

2013年11月，Go 1.2 的 FAQ 中增加了一条关于包版本管理的基本建议（此建议直到 Go 1.10 都没变过）：

> 发布到公共使用的包，在演进时应尽量保持向后兼容性（如 v1.5 应兼容 v1.4）。[Go 1 的兼容性指南](https://golang.org/doc/go1compat.html)就是一个很好的参考：不要移除已暴露的命名，鼓励使用组合单词来命名，等等。若需不同的功能，请创建一个新的命名，而非直接修改旧名字下的功能。若必须引入不兼容的修改，请创建一个新的包，并使用新的 `import` 路径。

2014年3月，Gustavo Niemeyer 创建了 [gopkg.in](https://gopkg.in/)，倡议在 Go 语言中编写稳定的 API。此网站可从 `go get` 的 URL 中解释出包的版本，并转发 GitHub 中对应版本的源码，如 `gopkg.in/yaml.v1`、`gopkg.in/yaml.v2` 分别对应 Git 仓库中的不同提交（也可能刚好是不同的分支）。基于语义的版本管理（semver），gopkg 希望包作者在引入不兼容性修改时创建一个新的主版本号，由于 `v2` 是完全不同的 API，所以 `v2` 的 `import` 路径可逐步替换 `v1` 的 `import` 路径（注：因为二者可被同时使用）。

**阶段三：Vendor 模式和可复现构建**
go get 的第二个致命问题：它无法保证构建是稳定可复现的，这意味着包的开发者和包的使用者很容易出现依赖不同版本的情况。

2013 年 11 月，Go 1.2的 FAQ 中还添加了这条基本建议：

> 使用一个外部的依赖包时，你若担心该包无故发生变化，最简单粗暴的解决办法是复制一份到你的本地仓库（Google 内部也是使用这种方法）。将副本存储在一个新的 `import` 路径中，将其标识为本地副本。例如，你可将 `original.com/pkg` 复制为 `you.com/external/original.com/pkg`。Keith Rarick 写了一个名为 `goven` 的工具将此过程自动化。

在这个阶段，各种第三方的 Go 依赖管理工具已经开始出现。它们都基于复制依赖的原理，通过非官方 Hack 的方式来完成可复现构建。

因此，Go 官方在看到这个乱象之后，正式推出官方 vendor 机制。vendor 目录用于存放依赖包，加载顺序是：vendor > GOROOT > GOPATH。vendor 于 Go 1.5进入实验阶段，于 Go 1.6 成为默认启动的功能。

从本质上说，vendor 机制只是包管理的一种不完整解决方案。它只提供了稳定可复现构建的能力，但并没有告诉项目具体用哪个版本。于是类似 glide、godep、govendor 等第三方包管理工具，通过添加特殊的版本信息记录文件，来做隐式包版本管理。但由于 Go 的官方工具链都无法识别类似的文件，因此导致 Go 工具链生态与之割裂。

**阶段四：官方包管理器实验**
在 GopherCon 2016 会议之后，Go 成立了一个包依赖管理器委员会和一个讨论小组，目的是创造一个新的依赖管理工具 [dep](https://github.com/golang/dep)。这个工具的愿景是统一目前各种 Go 的包管理器，继续使用 vendor 目录，但依然不属于 Go 官方工具链的原生功能。

引用 Russ Cox 在博客中所说：

> dep 有几大作用：
>
> -   它是今天为止可用的工具实践中的重要进步
> -   它是朝一个解决方案前进的重要一步
> -   它也是“官方实验”，试验哪些功能对 Go 开发者有用或没用
>
> 但 dep 不是 go 命令中整合版本管理的直接原型。dep 是一个强大的、很灵活的工具，探索了包管理的设计空间，在我们争论着如何对 Go 程序进行构建时，扮演着类似 Makefile 的角色。但当我们对包管理的设计空间了解得更深、明白到哪些关键特性必须实现后，我们才知道如何在 Go 生态系统中移除那些不必要的功能、灵活性。然后采取强制的约定，使得 Go 的代码库更统一、更易于理解、相关工具更易于构建。

**阶段五：vgo 提案和最终的官方解决方案 Go Modules**
dep 终究没有被官方采纳，虽然 Russ Cox 肯定了 dep 的历史作用，但 dep 本质上仍是 vendor 机制下的延伸，并且 dep 坚持语义版本，而并未使用 Go Modules 方案中的语义导入版本。因此，vgo 提案正式提出。

vgo 由 Russ Cox 于 2018 年提出，它建议采用语义导入版本规则结合最小版本选择规则。另一方面，vgo 希望能够与 Go 官方工具链相结合，因此最初以 “vgo” 命名，并且实验性阶段以替代 go 命令的方式运行。

vgo 很快得到官方认可，并作为正式 proposal 开发合入主干，最终以 go mod 命令正式纳入官方工具链。

Go Modules 去除 GOPATH 和 vendor 目录的依赖，不再需要基于复制依赖的做法，大大减少源码包的体积并杜绝了修改 vendor 目录内容的行为。另一方面，Go Modules 还集成进了 go 命令中，配合 go get、go list、go build 等等命令协同工作，整体体验更加优秀。

至此，Go 结束了漫长的依赖管理之争，最终形成大一统局面。但另一方面，vgo 的发布引发了社区的强烈动荡，以 dep 为首的开发者为此感到沮丧 [1](https://www.kevinwu0904.top/blogs/golang-modules/#fn:1)。

### Go Modules

**语义版本 vs 语义导入版本**
**语义版本**（semantic versioning）是指在 Go 语言中对同一个依赖的不同兼容性版本使用统一的 import 名称，即相同的 import 路径，在辅助文件中记录版本信息。语义版本的代表是 dep，例如：

```go
import (
	"github.com/robfig/cron"
)
```

```toml
# Gopkg.toml 版本1.2.0
[[constraint]]
  name = "github.com/robfig/cron"
  version = "1.2.0"
  
# Gopkg.toml 版本3.0.1
[[constraint]]
  name = "github.com/robfig/cron"
  version = "3.0.1"
```

**语义导入版本**（semantic import versioning）则是指在 Go 语言中对同一个依赖的不同兼容性版本使用不同的 import 名称，即不同的 import 路径，同时也在辅助文件中记录版本信息。导入版本的代表是 go module，例如：

-   v1大版本

```go
import "github.com/robfig/cron" // v1大版本
```

```go
// go.mod
require (
	github.com/robfig/cron v1.2.0
)
```

-   v3大版本

```go
import "github.com/robfig/cron/v3" // v3大版本
```

```go
// go.mod
require (
	github.com/robfig/cron v3.0.1
)
```

**import兼容性规则**
Go语言的import兼容性规则是指：

> 如果一个旧包与新包共用相同的 import 路径，那么新包必须向后兼容旧包。

Go的开发者采用业界通用的semantic versioning作为版本标识符，它形如（vmajor.minor.patch）：
![semver](/img/golang/semver.png)

- Major version：主版本，通常是大版本升级，导致向前不兼容
- Minor version：次版本，通常是向下兼容的 feture
- Patch version：修订版本，如一些 bugfix

Go 的兼容性规则希望开发者遵守如下守则：

1.  相同大版本，新包需要完全兼容旧包（通过诸如不删除弃用方法，不修改导出变量等手段）。例如：v1.3.1需要完全兼容v1.2.0。
2.  不同大版本，允许存在不兼容改动。例如：v3.0.0和v1.0.0允许声明不兼容的接口方法。
3.  v0.0.0到v1.0.0之间由于处于开发阶段，允许存在大量不兼容的行为，但需要开发者自己处理。

**最小版本选择（MVS）**
[**Minimal Version Selection**](https://research.swtch.com/vgo-mvs) 是 go mod 所实现的关于依赖升级和降级的选择策略。它的工作方式是这样的：我们为每个模块指定的依赖都是可用于构建的最低版本，最后实际选择的版本是所有出现过的最低版本中的最大值。

我们现在有这样的一个依赖关系，A 会依赖 B，C，而 B，C 会去依赖 D，D 又会依赖 E：
![mvs](/img/golang/mvs-1.png)
把每个模块依赖的版本都找出来，这样我们会首先得到一个粗略的清单。然后相同的模块我们总是取最大的版本，这样就能得到最终的依赖列表：
![version-select-list](/img/golang/version-select-list.png)
go mod 中使用的算法有 BuildList、Req、Upgrade、Downgrade 等。

**go.mod 和 go.sum**
go mod 在工程中引入 go.mod 和 go.sum 文件，其中 go.mod 文件用于记录依赖的版本信息，go.sum 文件用来记录依赖的 hash 值。

go.mod 文件示例：

```go
module example.com/my/thing

go 1.12

require example.com/other/thing v1.0.2
require example.com/new/thing/v2 v2.3.4
exclude example.com/old/thing v1.2.3
replace example.com/bad/thing v1.4.5 => example.com/good/thing v1.4.5
retract [v1.9.0, v1.9.5]
```

- require：require 记录的内容也即根据 MVS 中算法二：Req 生成的列表
- exclude：exclude 可以记录一个版本的黑名单，防止该版本被引用
- replace：replace 可以用于替换一个依赖
- retract：go1.16 新特性，可以用于包的开发者紧急撤回某个已知 BUG 的版本

go.sum 考虑的是依赖防篡改的问题。第一次拉取包含 go.sum 的工程，项目运行之后通常会去拉取依赖包。此时会通过 sha256（内容摘要算法）计算依赖包的 hash 值，并与 go.sum 记录值进行比较。go.sum 期望行为是 Append-Only 配合工具生成。

**命令行与工作流**
go mod 目前也是 Go 官方工具链中的一员，go mod 的常用工作流大致如下：

1.  创建新的 module：`go mod init` 首次接入 go mod 的项目可以通过 init 命令完成，init 命令会解析项目源码中 go 文件的 import 信息，并根据 MVS 相关算法，计算出相关依赖并最终生成 go.mod 和 go.sum。
2.  列出当前 module 下的所有依赖：`go list -m -json all`
3.  升级/降级一个依赖到指定版本：`go get -u github.com/pkg/errors@v0.9.0`
4.  升级所有依赖：`go get -u all`
5.  下载依赖包：`go mod download`
6.  下载依赖包并且精简 mod 和 sum 的内容：`go mod tidy`

go1.12 默认开启 go Module 功能，go1.12 会在用户的 \$home 目录下创建一个 go 文件夹作为默认的 GOPATH。go get 会将远程的软件包 download 在 \$home/go/pkg/mod 目录里。

以上为本文整理的全部资料，其中大部分内容来自于 [深入解析Go Modules | 极客熊生](https://www.kevinwu0904.top/blogs/golang-modules/)，讲解清晰，推荐大家看看。

## 参考资料

[1]  [Golang Package 与 Module 简介](https://www.jianshu.com/p/07ffc5827b26)
[2]  [理解Golang包导入，import、包名、目录名的关系](https://www.cnblogs.com/maji233/p/11045166.html)
[3]  [Go Modules 内部分享](https://xuanwo.io/2019/05/27/go-modules/)
[4]  [一文搞懂 Go Modules 前世今生及入门使用](https://www.cnblogs.com/wongbingming/p/12941021.html)
[5]  [深入解析Go Modules | 极客熊生](https://www.kevinwu0904.top/blogs/golang-modules/)
