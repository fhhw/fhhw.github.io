---
title: Java 技术体系
date: 2023-08-31 23:27:21
tags: Java
categories: Java
description: JCP 官方所定义的 Java 技术体系包括了以下几个组成部分：Java 程序设计语言、各种硬件平台上的 Java 虚拟机、Class 文件格式、Java API 类库、商业机构和开源社区的第三方 Java 类库。
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

**JDK 1.0 发布**

1996年1月23日，JDK 1.0发布，Java 语言有了第一个正式版本的运行环境。JDK 1.0提供了一个纯解释执行的 Java 虚拟机实现（Sun Classic VM）。JDK 1.0版本的代表技术包括：**Java 虚拟机、Applet、AWT 等**。

**JDK 1.1发布**

1997年2月19日，Sun 公司发布了 JDK 1.1，Java 里许多最基础的技术支撑点（如 JDBC 等）都是在 JDK 1.1版本中提出的，**JDK 1.1版的技术代表有：JAR 文件格式、JDBC、JavaBeans、RMI 等。Java 语言的语法也有了一定的增强，如内部类（Inner Class）和反射（Reflection）都是在这时候出现的**。

**JDK 1.2发布**

1998年12月4日，JDK 迎来了一个里程碑式的重要版本：工程代号为 Playground（竞技场）的 JDK 1.2。

Sun 在这个版本中把 Java 技术体系拆分为三个方向，分别是面向桌面应用开发的 J2SE（Java 2 Platform，StandardEdition）、面向企业级开发的 J2EE（Java 2 Platform，Enterprise Edition）和面向手机等移动终端开发的 J2ME（Java 2 Platform，Micro Edition）。

> J2EE 在 JDK 10以后被 Oracle 认为是一个不赚钱的业务，捐献给 Eclipse 基金会管理，但是不允许使用 Java 商标，所以改称为 Jakarta EE。

在这个版本中出现的代表性技术非常多，**如 EJB、Java Plug-in、Java IDL、Swing 等，并且这个版本中 Java 虚拟机第一次内置了 JIT（Just In Time）即时编译器**（JDK 1.2中曾并存过三个虚拟机，Classic VM、HotSpot VM 和 Exact VM，其中 Exact VM 只在 Solaris 平台出现过；后面两款虚拟机都是内置了 JIT 即时编译器的，而之前版本所带的 Classic VM 只能以外挂的形式使用即时编译器）。**在语言和 API 层面上，Java 添加了 strictfp 关键字，Java 类库添加了现在 Java 编码之中极为常用的一系列 Collections 集合类等。**

**JDK 1.3发布**

2000年5月8日，工程代号为 Kestrel（美洲红隼）的 JDK 1.3发布。**相对于 JDK1.2，JDK 1.3的改进主要体现在 Java 类库上（如数学运算和新的 Timer API 等），JNDI 服务从 JDK 1.3开始被作为一项平台级服务提供（以前 JNDI 仅仅是一项扩展服务），使用 CORBA IIOP 来实现 RMI 的通信协议，等等**。这个版本还对 Java 2D 做了很多改进，提供了大量新的 Java 2D API，并且新添加了 JavaSound 类库。

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

JDK10以后，Oracle 选择把 J2EE“扫地出门”，所有权直接赠送给 Eclipse 基金会，唯一的条件是以后不准再使用“Java”这个商标[插图]，所以取而代之的将是 Jakarta EE。

**JDK 11发布**

2018年9月25日，JDK 11发布，这是一个 LTS 版本的 JDK，包含17个 JEP，其中有 ZGC 这样的革命性的垃圾收集器出现，也有把 JDK 10中的类型推断加入 Lambda 语法这种可见的改进。

**JDK 12发布**

2019年3月20日，JDK 12发布，只包含8个 JEP，其中主要有 Switch 表达式、Java 微测试套件（JMH）等新功能，最引人注目的特性无疑是加入了由 RedHat 领导开发的 Shen-andoah 垃圾收集器。

Shenandoah 作为首个由非 Oracle 开发的垃圾收集器，其目标又与 Oracle 在 JDK 11中发布的 ZGC 几乎完全一致，两者天生就存在竞争。Oracle 马上用实际行动抵制了这个新收集器，在 JDK 11发布时才说应尽可能保证 OracleJDK 和 OpenJDK 的兼容一致，转眼就在 OracleJDK 12里把 Shenandoah 的代码通过条件编译强行剔除掉，使其成为历史上唯一进入了 OpenJDK 发布清单，但在 OracleJDK 中无法使用的功能。

## Java 三条产品线

Sun 公司基于商业上的考量把 Java SDK 划分成 SE、ME、EE 三个版本。

![Java 三条产品线之间的关系](/img/java/SEMEEE.PNG)

- **Java SE（J2SE，Java 2 Platform Standard Edition，标准版）**

![JDK Architecture](/img/java/jdk.webp)

Java SE 之所以称之为“标准版”，涉及两个责任，一个责任是 Java SE 中的虚拟机、Java 语言、编译器、核心类库，它们是 Java ME、Java EE 的共同基础，对其起着支撑作用。

第二个责任是，即使不提对Java ME、Java EE的支持，Java SE本身也可以完成一些“标准”程序的开发，例如桌面程序，控制台程序等。

作为第一个责任，Java SE 的基础支撑作用当然无可争议，所以，在 Java SE 阶段的学习中，了解虚拟机原理，会使用编译器，掌握 Java 语法及核心类库是必须完成的。

但就 Java SE 的第二个责任来说，Java SE 则没有完成使命，主要原因是因为其在桌面开发市场上受到了微软的猛烈阻击。因为 Windows 在桌面市场上具有统治性的地位，而其拥有者微软又为开发者提供了一套从上到下、非常优秀的工具包（包括 C#、WPF、Visual Studio 等）。与之相比，Java SE 自带的 UI 框架（例如 AWT、Swing）则是显得如此的简陋和不堪。这也难怪，作为 Window 的拥有者，又有谁比微软更懂桌面开发的呢？

今天，就工业开发的现状来说，Java在事实上已经退出了桌面开发市场了，与之相关的，Java SE中与桌面相关的类库和框架也就失去了继续学习的意义。

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

## Java EE

### Java EE 层级架构
![Java EE 架构](/img/java/Java_EE_architecture.webp)

总体上来说，Java EE 是一个 3 层架构：表现层、业务层、数据访问层。**Java EE 的核心是 JSP、Servlet、EJB**。

### The Java EE 7 stack

[jakarta ee - A summary of all Java EE specifications - Stack Overflow](https://stackoverflow.com/questions/37082364/a-summary-of-all-java-ee-specifications) 总结了 Java EE 的规范。

![Java EE 7 Stack](/img/java/Java_EE_7_stack.PNG)

![Java EE Architecture Diagram](/img/java/Java_EE_Architecture_Diagram.PNG)

### Java EE 十三种常用的核心技术规范：

- **JDBC (Java Database Connectivity)**
JDBC API 为访问不同的数据库提供了一种统一的途径，象 ODBC 一样，JDBC 对开发者屏蔽了一些细节问题，另外，JDBC 对数据库的访问也具有平台无关性。

- **JNDI (Java Name and Directory Interface)**
JNDI API 被用于执行名字和目录服务。它提供了一致的模型来存取和操作企业级的资源如 DNS 和 LDAP，本地文件系统，或应用服务器中的对象。

- **EJB (Enterprise JavaBean)**
J 2 EE 技术之所以赢得媒体广泛重视的原因之一就是 EJB。它们提供了一个框架来开发和实施分布式商务逻辑，由此很显著地简化了具有可伸缩性和高度复杂的企业级应用的开发。EJB 规范定义了 EJB 组件在何时如何与它们的容器进行交互作用。容器负责提供公用的服务，例如目录服务、事务管理、安全性、资源缓冲池以及容错性。但这里值得注意的是，EJB 并不是实现 J 2 EE 的唯一途径。正是由于 J 2 EE 的开放性，使得有的厂商能够以一种和 EJB 平行的方式来达到同样的目的。

- **RMI (Remote Method Invoke)**
调用远程对象上的方法。它使用了序列化方式在客户端和服务器端传递数据。RMI 是一种被 EJB 使用的更底层的协议。

- **Java IDL/CORBA**
在 Java IDL 的支持下，开发人员可以将 Java 和 CORBA 集成在一起。他们可以创建 Java 对象并使之可在 CORBA ORB 中展开，或者他们还可以创建 Java 类并作为和其它 ORB 一起展开的 CORBA 对象的客户。后一种方法提供了另外一种途径，通过它 Java 可以被用于将你的新的应用和旧的系统相集成。

- **JSP (Java Server Pages)**
JSP 页面由 HTML 代码和嵌入其中的 Java 代码所组成。服务器在页面被客户端所请求以后对这些 Java 代码进行处理，然后将生成的 HTML 页面返回给客户端的浏览器。

- **Java Servlet**
Servlet 是一种小型的 Java 程序，它扩展了 Web 服务器的功能。作为一种服务器端的应用，当被请求时开始执行，这和 CGI Perl 脚本很相似。Servlet 提供的功能大多与 JSP 类似，不过实现的方式不同。JSP 通常是大多数 HTML 代码中嵌入少量的 Java 代码，而 servlets 全部由 Java 写成并且生成 HTML。

- **XML (Extensible Markup Language)**
XML 是一种可以用来定义其它标记语言的语言。它被用来在不同的商务过程中共享数据。
XML 的发展和 Java 是相互独立的，但是，它和 Java 具有的相同目标正是平台独立性。通过将 Java 和 XML 的组合，您可以得到一个完美的具有平台独立性的解决方案。

- **JMS (Java Message Service)**
JMS 是用于和面向消息的中间件相互通信的应用程序接口（API）。它既支持点对点的域，又支持发布/订阅（publish/subscribe）类型的域，并且提供对下列类型的支持：经认可的消息传递，事务型消息的传递，一致性消息和具有持久性的订阅者支持。JMS 还提供了另一种方式来对您的应用与旧的后台系统相集成。

- **JTA (Java Transaction Architecture)**
JTA 定义了一种标准的 API，应用系统由此可以访问各种事务监控。

- **JTS (Java Transaction Service)**
JTS 是 CORBA OTS 事务监控的基本的实现。JTS 规定了事务管理器的实现方式。该事务管理器是在高层支持 Java Transaction API (JTA）规范，并且在较底层实现 OMG OTS specification 的 Java 映像。JTS 事务管理器为应用服务器、资源管理器、独立的应用以及通信资源管理器提供了事务服务。

- **JavaMail**
JavaMail 是用于存取邮件服务器的 API，它提供了一套邮件服务器的抽象类。不仅支持 SMTP 服务器，也支持 IMAP 服务器。

- **JAF (JavaBeans Activation Framework)**
JavaMail 利用 JAF 来处理 MIME 编码的邮件附件。MIME 的字节流可以被转换成 Java 对象，或者转换自 Java 对象。大多数应用都可以不需要直接使用 JAF。

### EJB 的缺陷

早期的 Java EE 平台（ J2EE ） 是推崇以 EJB 为核心的开发方式，这种方式存在以下几个弊端：

1. 没有面向实际问题：J2EE 和 EJB 的很多问题都源自它们“以规范为驱动”的本质。标准委员会所指定的规范，并没有针对性地解决问题，反而在实际开发中引人很多复杂性。毕竟，成功的标准都是从实践中发展来的，而不是由哪个委员会创造出来的。

2. 应反“帕累托法则”： “ 帕累托法则”也称“二八定律”，是指花较少的（ 10%～20% ）的成本解决大部分问题（ 80%～90% ），而架构的价值在于为常见的问题找到好的解决方案，而不是一心想要解决更复杂、也更为罕见的问题。 EJB 的问题就在于，它违背了这个法则一一为了满足少数情况下的特殊要求，它给大多数使用者强加上了不必要的复杂性。

3. 引人重复代码：大多数 J2EE 代码生成工具所生成的代码都是用于实现 J2EE 经典架构的，这会导致引人很多重复代码、过渡工程等问题。

4. 目标定位不清晰：早期的 EJB 2.1 规范中 EJB 的目标定位有 11 项之多 。而这些目标，没有一项是致力于简化 Java EE 开发的。

5. 编程模型复杂： EJB 组成复杂，要使用 EJB 需要继承非常多的接口。而这些接口，在实际开发中并不是真正为了解决问题。

6. 开发周期长： EJB 依赖于容器，所以 EJB 在编写业务逻辑时，是与容器耦合的。这必然就导致开发、测试、部署的难度增大。同时，也拉长了整个开发的周期。

7. 移植困难：规范中定义的目标是“Write Once, Run Anywhere”，但实际上这基本是一句空话。结果变成了一次编写，到处重写。特别是实体 Bean ，基本上迁移了一个服务器，就相当于需要重新编写，相应的测试工作量也增加了。规范中对实体映射的定义太过于宽泛，导致每个厂商都有自己的 ORM 实现，引入特定厂商的部署描述符，又因为 J 2 EE 中除 Web 外，类加载的定义没有明确，导致产生了特定厂商的类加载机制和打包方式。同时，特定厂商的服务查找方式也是有差异的。

当然，Java EE 也在持续改进现代软件所需的 API 和编程模型，并创建其社区所需的功能。其中一些功能包括异步 CDI 事件、增强的 JSON 支持、新的 REST 反应式客户端 API 等等。EJB 也在不断发展，解决了上述提到的一些问题。

## Spring 框架的诞生

面对 Java EE 的沉重，大量对 Java EE 不满（尤其是 EJB）的“草根”程序员们，集结在开源社区下，发起一场旨在使用“轻量级”技术以代替复杂 EJB 的运动。其中，最耀眼的两颗明星正是今天大名鼎鼎的 Spring 和 Hibernate。

“ Spring 之父” Rod Johnson 对传统的 Java EE 系统框架臃肿、低效、脱离现实的种种现状提出了质疑，并积极寻求探索革新。 2005年，Rod Johnson 的新书《J2EE Development without EJB》出版。在中这本书中，Rod Johnson 不仅列举了 EJB 的困境，而且还在理论上阐释了如何破解困境，然后基于理论，推出了具体的 Spring 和 Hibernate 作为终极解决方案。

EJB困境的根源在于复杂，因此要解决EJB的困境，必须让对象回归本质，用POJO（简单Java对象）替代EJB，用Spring代替EJB容器。依赖Spring的IOC（控制发展）、DI（依赖注入）、AOP（面向切面编程）能力，来消解EJB容器承担的诸多功能。其次，单独引入Hibernate，来解决EJB想解决，但从未成功的对象持久化问题。

后来的实践证明，Spring + Hibernate 的轻量组合，不仅完全可以替代 EJB，而且还具备简单高效的额外优点。所以很快，在 Java EE 开发中，Spring + Hibernate 组合就完全取代了 EJB，成为事实上的标准。

虽然 Spring 喊出了“Without EJB”的口号。实际上 Spring 是对 Java EE 的改进和补充。 Spring 本身也集成非常多的 Java EE 平台规范，如 Servlet API ( JSR 340 ）、WebSocket API ( JSR 356 ）、Concurrency Utilities ( JSR 236 ）、JSON Binding API ( JSR 367 ）、Bean Validation ( JSR 303 ） 、JPA ( JSR 338 ）、JMS ( JSR 914 ）、Dependency Injection (JSR 330 ）、Common Annotations ( JSR 250 ）等。

Spring 框架组件：

![Spring Framework Overview](/img/java/spring-overview.png)

Spring 的主要特性：

1. 轻量级 IoC 容器：IoC 容器是用于管理所有 bean 的声明周期，是 Spring 的核心组件。在此基础之上，开发者可以自行选择要集成的组件，如消息传递、事务管理、数据持久化及 Web 组件等。
2. 采用 AOP 编程方式：Spring 推崇使用 AOP 编程方式。 AOP ( Aspect Oriented Programming，面向切面编程）的目标与 OOP ( Object Oriented Programming ，面向对象编程）的目标并没有不同，都是为了减少重复和专注于业务。
3. 大量使用注解： Spring 提供了大量的注解，支持声明式的注入方式，极大地简化了配置。
4. 避免重复 “造轮子”：Spring 集成了大量市面上成熟的开源组件，站在巨人的肩膀上，这样既增强了 Spring 的功能，又避免了重复 “造轮子” 。

相比于经典的 Java EE 架构：
1. 前端：applet 被 ajax 替代，Java 桌面程序式弱，浏览器成为主流。
2. 中间：web 服务器的职责进一步清晰，仅承担服务前端的职责，同时轻量级的 Spring 容器也被引入。
3. 后端：1）应用服务器以 web service 的形式被外部调用，web 容器被引入；2）spring 容器替换了沉重的 EJB 容器；3）引入 Hibernate 来完成 Bean 的持久化。

随着分布式技术、微服务、前后端分离等理念的提出，Java EE 进最终演变成今天的样子：

1. 因为前后端分离，Web 服务器不再负责页面的渲染，JSP 被淘汰。
2. MVC 被引入，先是 Structs，后 Structs 被 Spring MVC 替代。
3. 更加敏捷的 MyBatis 越来越受欢迎，成为和 Hibernate 并立的两个主流持久化框架。
4. 分布式架构成为主流，原来位于应用服务器的功能组件被拆分到单独的服务器中，从而使应用服务器的定位更加明晰。

![Software Framework](/img/java/introduction-software-framework.png)

## Java 技术路线

![Java Development Roadmap](/img/java/JavaRoadmap2023.jpg)
## 参考资料

[1] 《深入理解 Java 虚拟机》- 周志明
[2]  [Java的前生今世（下） - 知乎](https://zhuanlan.zhihu.com/p/351843840)
[3]  Java EE 7 Essentials
[4]  [J2EE的13种核心技术 - bcombetter - 博客园](https://www.cnblogs.com/xingzc/p/5759915.html)
[5]  Java™ Platform, Enterprise Edition  (Java EE) Specification, v7
[6]  《Spring 5 开发大全》- 柳伟卫
[7]  [Spring Framework Tutorial for Beginners - Learn Spring - DataFlair](https://data-flair.training/blogs/spring-framework-tutorial/)
[8]  [GitHub - devoxx/JavaRoadmap: The 2023 Java Developers Roadmap](https://github.com/devoxx/JavaRoadmap)
