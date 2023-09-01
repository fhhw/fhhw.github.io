---
title: Java 发展历史
date: 2023-08-31 16:58:13
tags: Java
categories: Java
description: 1996年1月23日，JDK 1.0发布，Java语言有了第一个正式版本的运行环境。JDK 1.0提供了一个纯解释执行的Java虚拟机实现（Sun Classic VM）。JDK 1.0版本的代表技术包括 Java 虚拟机、Applet、AWT等。
---

## Java 技术体系

JCP 官方所定义的 Java 技术体系包括了以下几个组成部分：

- Java 程序设计语言
- 各种硬件平台上的 Java 虚拟机
- Class 文件格式
- Java API 类库
- 商业机构和开源社区的第三方 Java 类库

> **JCP**（**Java Community Process**）成立于1998年，是使有兴趣的各方参与定义[Java](https://zh.wikipedia.org/wiki/Java)的特征和未来版本的正式过程。
>
> JCP 使用 **JSR**（Java 规范请求，*Java Specification Requests*）作为正式规范文档，描述被提议加入到 Java 体系中的的规范和技术。

> The JCP process defines three key deliverables for any JSR: 
>
> **Specification** 
> A formal document that describes the proposed component and its features.
>
> **Reference Implementation (RI)** 
> Binary implementation of the proposed specification. The RI helps to ensure that the proposed specifications can be implemented in a binary form and provides constant feedback to the specification process. 
> The RI of Java EE is built in the GlassFish community. 
>
> **Technology Compliance Kit (TCK)** 
> A set of tests that verify that the RI is in compliance with the specification. This allows multiple vendors to provide compliant implementations.

## Java 的发展历史

![Java 发展历史](/img/java/Java_history.png)

**JDK 1.0发布**

1996年1月23日，JDK 1.0发布，Java语言有了第一个正式版本的运行环境。JDK 1.0提供了一个纯解释执行的Java虚拟机实现（Sun Classic VM）。JDK 1.0版本的代表技术包括：**Java虚拟机、Applet、AWT等**。

**JDK 1.1发布**

1997年2月19日，Sun公司发布了JDK 1.1，Java里许多最基础的技术支撑点（如JDBC等）都是在JDK 1.1版本中提出的，**JDK 1.1版的技术代表有：JAR文件格式、JDBC、JavaBeans、RMI等。Java语言的语法也有了一定的增强，如内部类（Inner Class）和反射（Reflection）都是在这时候出现的**。

**JDK 1.2发布**

1998年12月4日，JDK迎来了一个里程碑式的重要版本：工程代号为Playground（竞技场）的JDK 1.2。

Sun在这个版本中把Java技术体系拆分为三个方向，分别是面向桌面应用开发的J2SE（Java 2 Platform，StandardEdition）、面向企业级开发的J2EE（Java 2 Platform，Enterprise Edition）和面向手机等移动终端开发的J2ME（Java 2 Platform，Micro Edition）。

> J2EE在JDK 10以后被Oracle认为是一个不赚钱的业务，捐献给Eclipse基金会管理，但是不允许使用Java商标，所以改称为Jakarta EE。

在这个版本中出现的代表性技术非常多，**如EJB、Java Plug-in、Java IDL、Swing等，并且这个版本中Java虚拟机第一次内置了JIT（Just In Time）即时编译器**（JDK 1.2中曾并存过三个虚拟机，Classic VM、HotSpot VM和Exact VM，其中Exact VM只在Solaris平台出现过；后面两款虚拟机都是内置了JIT即时编译器的，而之前版本所带的Classic VM只能以外挂的形式使用即时编译器）。**在语言和API层面上，Java添加了strictfp关键字，Java类库添加了现在Java编码之中极为常用的一系列Collections集合类等。**

**JDK 1.3发布**

2000年5月8日，工程代号为Kestrel（美洲红隼）的JDK 1.3发布。**相对于JDK1.2，JDK 1.3的改进主要体现在Java类库上（如数学运算和新的Timer API等），JNDI服务从JDK 1.3开始被作为一项平台级服务提供（以前JNDI仅仅是一项扩展服务），使用CORBA IIOP来实现RMI的通信协议，等等**。这个版本还对Java 2D做了很多改进，提供了大量新的Java 2D API，并且新添加了JavaSound类库。

**JDK 1.4发布**

2002年2月13日，JDK 1.4发布，工程代号为Merlin（灰背隼）。JDK 1.4是标志着Java真正走向成熟的一个版本，Compaq、Fujitsu、SAS、Symbian、IBM等著名公司都有参与功能规划，甚至实现自己独立发行的JDK 1.4。哪怕是在近二十年后的今天，仍然有一些主流应用能直接运行在JDK 1.4之上，或者继续发布能运行在1.4上的版本。**JDK 1.4同样带来了很多新的技术特性，如正则表达式、异常链、NIO、日志类、XML解析器和XSLT转换器，等等**。

**JDK 1.4发布**

2002年2月13日，JDK 1.4发布，工程代号为 Merlin（灰背隼）。JDK 1.4是标志着 Java 真正走向成熟的一个版本，Compaq、Fujitsu、SAS、Symbian、IBM 等著名公司都有参与功能规划，甚至实现自己独立发行的 JDK 1.4。哪怕是在近二十年后的今天，仍然有一些主流应用能直接运行在 JDK 1.4之上，或者继续发布能运行在1.4上的版本。**JDK 1.4同样带来了很多新的技术特性，如正则表达式、异常链、NIO、日志类、XML 解析器和 XSLT 转换器，等等**。

**JDK 5发布**

2004年9月30日，JDK 5发布，工程代号为 Tiger（老虎）。Sun 公司从这个版本开始放弃了谦逊的“JDK 1.x”的命名方式，将产品版本号修改成了“JDK x”。**从 JDK 1.2以来，Java 在语法层面上的变动一直很小，而 JDK 5在 Java 语法易用性上做出了非常大的改进。如：自动装箱、泛型、动态注解、枚举、可变长参数、遍历循环（foreach 循环）等语法特性都是在 JDK 5中加入的。在虚拟机和 API 层面上，这个版本改进了 Java 的内存模型（Java Memory Model，JMM）、提供了 java.util.concurrent 并发包等**。另外，JDK 5是官方声明可以支持 Windows 9x 操作系统的最后一个 JDK 版本。

**JDk 6发布**

2006年12月11日，JDK 6发布，工程代号为 Mustang（野马）。

在这个版本中，Sun 公司终结了从 JDK 1.2开始已经有八年历史的 J2EE、J2SE、J2ME 的产品线命名方式，启用 Java EE 6、Java SE 6、Java ME 6的新命名来代替。

**JDK 6的改进包括：提供初步的动态语言支持（通过内置 Mozilla JavaScript Rhino 引擎实现）、提供编译期注解处理器和微型 HTTP 服务器 API，等等。同时，这个版本对 Java 虚拟机内部做了大量改进，包括锁与同步、垃圾收集、类加载等方面的实现都有相当多的改动**。

> JDK 6发布后，Sun 公司把 Java 开源了，形成了 OpenJDK 项目。OpenJDK 和 JDK 的代码基本一致，除了极少量的产权代码（Encumbered Code，这部分代码所有权不属于 Sun 公司，Sun 本身也无权进行开源处理）外，OpenJDK 几乎拥有了当时 JDK 的全部代码。
>
> 我们可以这样简单理解两者的关系。JDK 的的代码是 master 主分支上的代码，openJDK 是从 master 上拉出来的一个产品线，在这个分支上删除了 Encumbered Code 等这些产权代码。所以如果你用不到这些产权特性的话，JDK 和 OpenJDK 可以认为是一致的。

**JDK 7发布**

在 JDK 7发布之前，Java 的老东家 Sun 公司被 Oracle 收购了。在原本计划在 JDK 7中发布的很多特性不能如期完成。

为了保证如期发布 JDK 7，Oracle 大幅裁剪了 JDK 7预定目标，以保证 JDK 7的正式版能够于2011年7月28日准时发布。主要措施是把不能按时完成的 Lambda 项目、Jigsaw 项目和 Coin 项目的部分改进延迟到 JDK 8之中。

最终，**JDK 7包含的改进有：提供新的 G1收集器（G1在发布时依然处于 Experimental 状态，直至2012年4月的 Update 4中才正式商用）、加强对非 Java 语言的调用支持（JSR-292，这项特性在到 JDK 11还有改动）、可并行的类加载架构等**。

**JDK 8发布**

2014年3月18日，JDK 8发布。从 JDK 8开始，Oracle 启用 JEP（JDK Enhancement Proposals）来定义和管理纳入新版 JDK 发布范围的功能特性。JDK 8提供了那些曾在 JDK 7中规划过，但最终未能在 JDK 7中完成的功能，主要包括：

- JEP 126：对 Lambda 表达式的支持，这让 Java 语言拥有了流畅的函数式表达能力。
- JEP 104：内置 Nashorn JavaScript 引擎的支持。
- JEP 150：新的时间、日期 API。
- JEP 122：彻底移除 HotSpot 的永久代。

以上只是列出了JDK 8的部分新特性。

**JDK 9发布**

2017年9月21日，JDK 9发布。JDK 9主要发布了 Jigsaw（支持 Java 模块化，对标 OSGi），除了 Jigsaw 外，JDK 9还增强了若干工具（JS Shell、JLink、JHSDB 等），整顿了 HotSpot 各个模块各自为战的日志系统，支持 HTTP 2客户单 API 等91个 JEP。

JDK 9以后，Oracle 宣布每六个 JDK 大版本中才会被划出一个长期支持（Long Term Support，LTS）版，只有 LTS 版的 JDK 能够获得为期三年的支持和更新，普通版的 JDK 就只有短短六个月的生命周期。**JDK 8和 JDK 11会是 LTS 版，再下一个就到2021年发布的 JDK 17了**。

**JDK 10发布**

2018年3月20日，JDK 10如期发布，这版本的主要研发目标是内部重构，诸如统一源仓库、统一垃圾收集器接口、统一即时编译器接口（JVMCI 在 JDK 9已经有了，这里是引入新的 Graal 即时编译器）等，这些都将会是对未来 Java 发展大有裨益的改进，但对普通用户来说 JDK 10的新特性就显得乏善可陈，毕竟它只包含了12个 JEP，而且其中只有本地类型推断这一个编码端可见的改进。

JDK10以后，Oracle 选择把 J2EE“扫地出门”，所有权直接赠送给 Eclipse 基金会，唯一的条件是以后不准再使用“Java”这个商标，所以取而代之的将是 Jakarta EE。

**JDK 11发布**

2018年9月25日，JDK 11发布，这是一个 LTS 版本的 JDK，包含17个 JEP，其中有 ZGC 这样的革命性的垃圾收集器出现，也有把 JDK 10中的类型推断加入 Lambda 语法这种可见的改进。

**JDK 12发布**

2019年3月20日，JDK 12发布，只包含8个 JEP，其中主要有 Switch 表达式、Java 微测试套件（JMH）等新功能，最引人注目的特性无疑是加入了由 RedHat 领导开发的 Shen-andoah 垃圾收集器。

Shenandoah 作为首个由非 Oracle 开发的垃圾收集器，其目标又与 Oracle 在 JDK 11中发布的 ZGC 几乎完全一致，两者天生就存在竞争。Oracle 马上用实际行动抵制了这个新收集器，在 JDK 11发布时才说应尽可能保证 OracleJDK 和 OpenJDK 的兼容一致，转眼就在 OracleJDK 12里把 Shenandoah 的代码通过条件编译强行剔除掉，使其成为历史上唯一进入了 OpenJDK 发布清单，但在 OracleJDK 中无法使用的功能。

## Java 三条产品线

Sun 公司基于商业上的考量把 Java SDK 划分成 SE、ME、EE 三个版本。

![Java 三条产品线之间的关系](/img/java/SEMEEE.PNG)

- **Java SE（J2SE，Java 2 Platform Standard Edition，标准版）**

Java SE 之所以称之为“标准版”，涉及两个责任，一个责任是 Java SE 中的虚拟机、Java 语言、编译器、核心类库，它们是 Java ME、Java EE 的共同基础，对其起着支撑作用。

第二个责任是，即使不提对Java ME、Java EE的支持，Java SE本身也可以完成一些“标准”程序的开发，例如桌面程序，控制台程序等。

作为第一个责任，Java SE 的基础支撑作用当然无可争议，所以，在 Java SE 阶段的学习中，了解虚拟机原理，会使用编译器，掌握 Java 语法及核心类库是必须完成的。

但就 Java SE 的第二个责任来说，Java SE 则没有完成使命，主要原因是因为其在桌面开发市场上受到了微软的猛烈阻击。因为 Windows 在桌面市场上具有统治性的地位，而其拥有者微软又为开发者提供了一套从上到下、非常优秀的工具包（包括 C#、WPF、Visual Studio 等）。与之相比，Java SE 自带的 UI 框架（例如 AWT、Swing）则是显得如此的简陋和不堪。这也难怪，作为 Window 的拥有者，又有谁比微软更懂桌面开发的呢？

今天，就工业开发的现状来说，Java 在事实上已经退出了桌面开发市场了，与之相关的，Java SE 中与桌面相关的类库和框架也就失去了继续学习的意义。

![Java SE JDK](/img/java/jdk.webp)

- **Java ME（J2ME，Java 2 Platform Micro Edition，微型版）**

Java ME 承载了 Java 最初的在小设备上运行的使命。

考虑到小设备的资源有限，Java ME 对 Java SE 的虚拟机做了优化，裁减了一部分类库，重新设计了一部分类库。

在今天这个智能机时代，Java ME 早已退出了历史舞台，不见了踪影。据此，有人认为 Java ME 是个失败的产品。但我认为这种说法并不公平，因为它忽略了历史的具体过程。

在功能机时代，不像今天，有两个居于统治地位的操作系统（Android、IOS）成为事实上的标准。当时的各个手机厂商（著名的有诺基亚、摩托罗拉、三星等）不仅有形态各异的硬件，还有各自完全不兼容的操作系统。所以在当时，如果没有 Java ME 作为各个品牌的出厂标配，应用软件的兼容性绝对是所有开发者的梦魇。

而正是因为 Java ME 的存在，才使得那个时代的功能机有了一丝“智能”的味道。如果有从功能机时代一路过来的小伙伴，一定还记得，当时在“玩机”论坛发布、寻找、交换 Java ME 小游戏带给我们的快乐。

至于后来 Java ME 为什么没有更进一步，拥有今天 Android 和 IOS 的市场地位，这可能就是一个语言的宿命。就好像我们虽知道 C++有这样那样的缺点，但我们依然不会尝试把它改造成类似 Java 这样的托管语言。如果是那样做的话，那就不再是一个升级的 C++，而是一门全新的语言了。

但无论如何，今天，对 Java 学习者来说，是对 Java ME 说再见的时刻了。至于对 Java ME 的评价，我认为最公允的是：Java ME 终于在完成它的使命后，光荣的退出了历史舞台。

- **Java EE（J2EE，Java 2 Platform Enterprise Edition，企业版）**

Java EE 的目标是用 Java 开发企业级软件。Java EE 由 SUN 牵头指定规范（标准），然后各大厂家根据规范具体实现。Oracle 公司将 Java EE 8 移交给了开源组织 Eclipse 基金会来进行管理，该平台改名为 Jakarta EE。

**Java EE 发展史**

| 版本             | 发布日期  | 焦点说明                                                     |
| ---------------- | --------- | ------------------------------------------------------------ |
| J2EE 1.4         | 2003.12   | **对Web服务更好支持**。启用javax命名空间。Servlet 2.4、JSP 2.0、EJB 2.1等 |
| Java EE 5        | 2006.05   | 以Web为着力点继续优化。Servlet 2.5、JSP 2.1、**EJB 3.0**、注解支持等 |
| Java EE 6        | 2009.12   | 添加了大量新技术来简化开发，如：**Servlet 3.0**(异步处理)、Bean Validation、EJB 3.1、JSF 2.0、JPA 2.0、**上下文和依赖注入(CDI)** |
| Java EE 7        | 2013.06   | 提高生产力满足企业需求和HTML5。Servlet 3.1、**WebSocket 1.0**、JSON 1.0、JMX 2.0、Batch 1.0 |
| **Java EE 8**    | 2017.08   | 增加了JSON绑定和安全相关。Servlet 4.0、Bean Validation 2.0、CDI 2.0、JPA 2.2 |
| `Jakarta EE入局` | `2017.08` | `Oracle将Java EE交给开源组织，Eclipse基金会接手（Apache基金会爆冷出局还是不想要？）。但Oracle不允许开源组织使用Java名号，所以Jakarta EE名称于2018.02.26应运而生` |
| **Jakarta EE 8** | 2019.09   | **规范与 Java EE 8完全相同**。Maven 的 GAV 变了：`javax.servlet:javax.servlet-api:4.0.1 -> jakarta.servlet:jakarta.servlet-api:4.0.2`，但命名空间没变依旧还是 javax.\*，算是个小过度吧 |
| Jakarta EE 9     | 2020.11   | **没有加入新功能**，Eclipse 基金会的首个正式版本。命名空间从 `javax.*` 迁移到 `jakarta.*`，前者从此成为历史。**所有模块**大版本号+1，如 `Servlet 4.0.2 -> Servlet 5` 以表示其断层式升级 |
| Jakarta EE 9.1   | 2021.06   | 相较于9 **没有** 加入新API。主要提供对Java SE 11的运行支持   |

## 参考资料

[1] 《深入理解 Java 虚拟机》- 周志明
[2]  [Java的前生今世（下） - 知乎](https://zhuanlan.zhihu.com/p/351843840)
[3]  [从Java EE到Jakarta EE，企业版Java的发展历程 | YourBatman](https://yourbatman.cn/x2y/3839cb84.html)
