---
title: Java EE 简介
date: 2023-09-01 17:04:00
tags: Java
categories: Java
cover: /img/java/javaee_logo.png
description: Java EE 的目标是用 Java 开发企业级软件。Java EE 由 SUN 牵头指定规范（标准），然后各大厂家根据规范具体实现。Oracle 公司将 Java EE 8 移交给了开源组织 Eclipse 基金会来进行管理，该平台改名为 Jakarta EE。
---

## Java EE 层级架构
![Java EE 架构](/img/java/Java_EE_architecture.webp)

总体上来说，Java EE 是一个 3 层架构：表现层、业务层、数据访问层。**Java EE 的核心是 JSP、Servlet、EJB**。

## The Java EE 7 stack

![Java EE Architecture Diagram](/img/java/Java_EE_Architecture_Diagram.PNG)

Java EE 的参考实现是：**Glassfish Application Server**。除此之外，流行的还有 TomEE、WebSphere、WebLogic、JBoss 等等。

### Web 容器

Web 容器是 Java EE 环境的一部分，专用于运行那些 Web 组件，如：pages 网页、JSP、Servlet、JSTL 及其他 Java EE Web 组件。Web 容器可通过标准 Web 连接到 Java EE 应用程序的**客户端**进行交互协议，当然更可以使用 Http、WebSocket 等公开协议。

在 Java EE 中，纯 Web 容器一般只有三种：Tomcat、Jetty、Undertow。其它的如 Glassfish、Weblogic 等属于应用服务器，包含了 Web 的功能。

Web 容器/服务器有着比应用服务器更加轻量级的特点，随着 Spring 成为主流技术也让轻量级的 Web 容器更加受到青睐，最具代表性的当属 Tomcat。

Tomcat 是 Apache 软件基金会的 Jakarta 项目中的一个**核心项目**。由 Apache、Sun 和其他一些公司及个人共同开发而成，由于有了 Sun 的参与（现在 Oracle）和支持，Tomcat 总是能最及时的支持到最新版的 Servlet/JSP 技术规范。

### EJB容器

EJB 容器是 Java EE 环境的一部分，专用于**运行 Java EE 应用程序**的应用程序逻辑部分。EJB 是包含和操纵 Java EE 应用程序的核心数据结构的 Java 类。

Spring 容器的功能跟它很类似。然后 EJB 容易由于它过重的设计，现在已经败下阵来，成为了 Spring 的天下。

值得强调的是：Tomca t 是不包含 EJB 容器的（无 Java EE 运行环境），不过他“哥哥”Tom EE 有，是个完整的应用服务器。
### Java EE 十三种常用的核心技术规范：


[jakarta ee - A summary of all Java EE specifications - Stack Overflow](https://stackoverflow.com/questions/37082364/a-summary-of-all-java-ee-specifications) 总结了 Java EE 的规范。

![Java EE 7 Stack](/img/java/Java_EE_7_stack.PNG)

- **JDBC (Java Database Connectivity)**
JDBC API 为访问不同的数据库提供了一种统一的途径，象 ODBC 一样，JDBC 对开发者屏蔽了一些细节问题，另外，JDBC 对数据库的访问也具有平台无关性。

- **JNDI (Java Name and Directory Interface)**
JNDI API 被用于执行名字和目录服务。它提供了一致的模型来存取和操作企业级的资源如 DNS 和 LDAP，本地文件系统，或应用服务器中的对象。

- **EJB (Enterprise JavaBean)**
J2EE 技术之所以赢得媒体广泛重视的原因之一就是 EJB。它们提供了一个框架来开发和实施分布式商务逻辑，由此很显著地简化了具有可伸缩性和高度复杂的企业级应用的开发。EJB 规范定义了 EJB 组件在何时如何与它们的容器进行交互作用。容器负责提供公用的服务，例如目录服务、事务管理、安全性、资源缓冲池以及容错性。但这里值得注意的是，EJB 并不是实现 J2EE 的唯一途径。正是由于 J2EE 的开放性，使得有的厂商能够以一种和 EJB 平行的方式来达到同样的目的。

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

## EJB 的缺陷

早期的 Java EE 平台（ J2EE ） 是推崇以 EJB 为核心的开发方式，这种方式存在以下几个弊端：

1. 没有面向实际问题：J2EE 和 EJB 的很多问题都源自它们“以规范为驱动”的本质。标准委员会所指定的规范，并没有针对性地解决问题，反而在实际开发中引人很多复杂性。毕竟，成功的标准都是从实践中发展来的，而不是由哪个委员会创造出来的。

2. 应反“帕累托法则”： “ 帕累托法则”也称“二八定律”，是指花较少的（ 10%～20% ）的成本解决大部分问题（ 80%～90% ），而架构的价值在于为常见的问题找到好的解决方案，而不是一心想要解决更复杂、也更为罕见的问题。 EJB 的问题就在于，它违背了这个法则一一为了满足少数情况下的特殊要求，它给大多数使用者强加上了不必要的复杂性。

3. 引人重复代码：大多数 J2EE 代码生成工具所生成的代码都是用于实现 J2EE 经典架构的，这会导致引人很多重复代码、过渡工程等问题。

4. 目标定位不清晰：早期的 EJB 2.1 规范中 EJB 的目标定位有 11 项之多。而这些目标，没有一项是致力于简化 Java EE 开发的。

5. 编程模型复杂： EJB 组成复杂，要使用 EJB 需要继承非常多的接口。而这些接口，在实际开发中并不是真正为了解决问题。

6. 开发周期长： EJB 依赖于容器，所以 EJB 在编写业务逻辑时，是与容器耦合的。这必然就导致开发、测试、部署的难度增大。同时，也拉长了整个开发的周期。

7. 移植困难：规范中定义的目标是“Write Once, Run Anywhere”，但实际上这基本是一句空话。结果变成了一次编写，到处重写。特别是实体 Bean ，基本上迁移了一个服务器，就相当于需要重新编写，相应的测试工作量也增加了。规范中对实体映射的定义太过于宽泛，导致每个厂商都有自己的 ORM 实现，引入特定厂商的部署描述符，又因为 J2EE 中除 Web 外，类加载的定义没有明确，导致产生了特定厂商的类加载机制和打包方式。同时，特定厂商的服务查找方式也是有差异的。

当然，Java EE 也在持续改进现代软件所需的 API 和编程模型，并创建其社区所需的功能。其中一些功能包括异步 CDI 事件、增强的 JSON 支持、新的 REST 反应式客户端 API 等等。EJB 也在不断发展，解决了上述提到的一些问题。
## Spring 框架的诞生

面对 Java EE 的沉重，大量对 Java EE 不满（尤其是 EJB）的“草根”程序员们，集结在开源社区下，发起一场旨在使用“轻量级”技术以代替复杂 EJB 的运动。其中，最耀眼的两颗明星正是今天大名鼎鼎的 Spring 和 Hibernate。

“ Spring 之父” Rod Johnson 对传统的 Java EE 系统框架臃肿、低效、脱离现实的种种现状提出了质疑，并积极寻求探索革新。 2005 年，Rod Johnson 的新书《J 2 EE Development without EJB》出版。在中这本书中，Rod Johnson 不仅列举了 EJB 的困境，而且还在理论上阐释了如何破解困境，然后基于理论，推出了具体的 Spring 和 Hibernate 作为终极解决方案。

EJB 困境的根源在于复杂，因此要解决 EJB 的困境，必须让对象回归本质，用 POJO（简单 Java 对象）替代 EJB，用 Spring 代替 EJB 容器。依赖 Spring 的 IOC（控制发展）、DI（依赖注入）、AOP（面向切面编程）能力，来消解 EJB 容器承担的诸多功能。其次，单独引入 Hibernate，来解决 EJB 想解决，但从未成功的对象持久化问题。

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

## 参考资料

[1]  Java EE 7 Essentials
[2]  [J2EE的13种核心技术 - bcombetter - 博客园](https://www.cnblogs.com/xingzc/p/5759915.html)
[3]  Java™ Platform, Enterprise Edition  (Java EE) Specification, v 7
[4]  [什么是Java EE？ | YourBatman](https://yourbatman.cn/x2y/9f826227.html)
[5]  《Spring 5 开发大全》- 柳伟卫
[6]  [Spring Framework Tutorial for Beginners - Learn Spring - DataFlair](https://data-flair.training/blogs/spring-framework-tutorial/)
