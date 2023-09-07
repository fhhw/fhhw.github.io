---
title: Maven自动化的构建工具
date: 2022-03-20 17:16:53
tags: Maven
categories: Java
cover: /img/java/maven_logo.png
description: Maven是Apache基金会的开源项目，使用Java语法开发，Maven是项目的自动化构建工具，管理项目的依赖。类似的构建工具还有：Ant，Gradle。
---

### 什么是Maven

Maven是Apache基金会的开源项目，使用Java语法开发，Maven是项目的自动化构建工具，管理项目的依赖。

类似的构建工具还有：Ant，Gradle

### Maven能做什么

（1）项目的自动构建，帮助开发人员做项目代码的编译、测试、打包、安装、部署等工作；

（2）管理依赖

依赖：项目中需要使用的其他资源，常见的是jar；
备注：没有Maven时，管理jar，需要从网络中单独下载，需要选择正确版本， 手工处理jar文件之间的依赖



### Maven中的概念
#### 约定的目录结构

| 目录                               | 目的                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| ${basedir}                         | 存放 pom.xml 和所有的子目录                                  |
| ${basedir}/src/main/java           | 项目的 Java 源代码                                           |
| ${basedir}/src/main/resources      | 项目的资源，比如说 property 文件，springmvc.xml              |
| ${basedir}/src/test/java           | 项目的测试类，比如说 Junit 代码                              |
| ${basedir}/src/test/resources      | 测试用的资源                                                 |
| ${basedir}/src/main/webapp/WEB_INF | web 应用文件目录，web 的项目信息，比如存放 web.xml、本地图片、jsp 视图页面 |
| ${basedir}/target                  | 打包输出目录                                                 |
| ${basedir}/target/classes          | 编译输出目录                                                 |
| ${basedir}/target/test-classes     | 测试编译输出目录                                             |
| Test.java                          | Maven 只会自动运行符合该命名规则的测试类                     |
| ~/.m2/repository                   | Maven 默认的本地仓库目录位置                                 |

#### 配置文件

| 类型                  | 在哪定义                                                     |
| --------------------- | ------------------------------------------------------------ |
| 项目级（per Project） | 定义在项目的 POM 文件 pom.xml 中                             |
| 用户级（Per User）    | 定义在 Maven 的设置 xml 文件中（%USER_HOME%/.m2/settings.xml） |
| 全局（Global）        | 定义在 Maven 的全局设置 xml 文件中（%MAVEN_HOME%/conf/settings.xml） |

#### POM
Project Object Model项目对象模型，Maven把项目当做模型处理。
Maven通过pom.xml文件实现项目的构建和依赖的管理。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- project是根标签，后面是约束文件 -->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <!-- POM模型的版本 -->
    <modelVersion>4.0.0</modelVersion>
    
    <!-- 坐标 -->
    <groupId>edu.stanford.cs.crypto</groupId>
    <artifactId>efficientct</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!-- 打包类型 -->
    <packaging>jar</packaging>
    
    <!-- Maven常用设置 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <!-- 设置构建项目的相关内容 -->
    <build>
        <!-- 设置插件 -->
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.0</version>
                <configuration>
                    <source>8</source><!-- 指定编译代码的jdk版本 -->
                    <target>8</target><!-- 指定运行代码的jdk版本 -->
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>sonatype-oss-snapshot</id>
            <snapshots/>
            <url>https://oss.sonatype.org/content/repositories/snapshots</url>
        </repository>
        <repository>
            <id>project.local</id>
            <name>project</name>
            <url>file:${project.basedir}/repo</url>
        </repository>
    </repositories>
</project>
```

（1）坐标

坐标组成是 groupId  artifactId   version
坐标的作用：确定资源，是资源的唯一标识，简称gav

```xml
 <groupId>edu.stanford.cs.crypto</groupId> groupId：组织名称，组织域名倒写
 <artifactId>efficientct</artifactId>      artifactId：项目名称
 <version>1.0-SNAPSHOT</version>           version：版本，项目的版本号，使用数字，例如1.0.3
```

> 备注：版本号中带-SNAPSHOT表示快照，为不稳定版本，Release 则代表稳定的版本。
>
> 项目中使用gav
>       1、每个Maven项目，都需要有一个自己的gav
>       2、管理依赖，需要使用其他jar，也需要使用gav作为标识

（2）依赖管理

```xml
 <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
 </dependencies>
```

#### 仓库管理

Maven仓库存放Maven工具jar包，第三方jar包，自己编写的jar包

仓库的分类：

1、本地（local）：
默认路径：C:\Users\Ivresse\\.m2\repository   

修改本地仓库位置，修改安装目录apache-maven-3.8.4\conf\settings.xml文件中<localRepository></localRepository>标签

2、中央（central）： Maven 社区提供的仓库
要浏览中央仓库的内容，maven 社区提供了一个 URL：[http://search.maven.org/#browse](http://search.maven.org/#browse)，使用这个仓库，开发人员可以搜索所有可以获取的代码库，或者使用[https://mvnrepository.com/](https://mvnrepository.com/)。
Maven 仓库默认在国外， 国内使用难免很慢，我们可以更换为阿里云的仓库，[配置指南](https://developer.aliyun.com/mvn/guide)。
修改 maven 根目录下的 conf 文件夹中的 settings.xml 文件，在 mirrors 节点上，添加内容如下：

```xml
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云公共仓库</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

如果想在项目中使用其它代理仓库，可在 **<repositories></repositories>** 节点中加入对应的仓库使用地址。以使用 spring 代理仓为例：

```xml
<repository>
  <id>spring</id>
  <url>https://maven.aliyun.com/repository/spring</url>
  <releases>
    <enabled>true</enabled>
  </releases>
  <snapshots>
    <enabled>true</enabled>
  </snapshots>
</repository>
```

全局配置则将上述配置写在<profile></profile>标签中

```xml
<settings>  
  ...  
  <profiles>  
    <profile>  
      <id>dev</id>  
      <!-- repositories and pluginRepositories here-->  
    </profile>  
  </profiles>  
  <activeProfiles>  
    <activeProfile>dev</activeProfile>  
  </activeProfiles>  
  ...  
</settings>  
```

3、远程（remote）：开发人员自己定制仓库     

当我们执行 Maven 构建命令时，Maven 开始按照以下顺序查找依赖的库：

- **步骤 1** － 在本地仓库中搜索，如果找不到，执行步骤 2，如果找到了则执行其他操作。
- **步骤 2** － 在中央仓库中搜索，如果找不到，并且有一个或多个远程仓库已经设置，则执行步骤 4，如果找到了则下载到本地仓库中以备将来引用。
- **步骤 3** － 如果远程仓库没有被设置，Maven 将简单的停滞处理并抛出错误（无法找到依赖的文件）。
- **步骤 4** － 在一个或多个远程仓库中搜索依赖的文件，如果找到则下载到本地仓库以备将来引用，否则 Maven 将停止处理并抛出错误（无法找到依赖的文件）。

#### 生命周期

项目构建的各个阶段，包括清理、编译、测试、打包、安装、部署。

#### 插件和命令

要完成构建项目的各个阶段，要使用Maven命令，执行命令的功能是通过插件完成的，插件就是jar包，一些类。

### Maven命令
```bash
mvn -v 
# 查看Maven版本信息

# 以下命令在项目根目录下执行
mvn clean
# 清理命令，删除以前生成的数据（删除target目录）
# 插件：maven-clean-plugin
mvn compile    
# 编译命令，将src/main/java目录中的代码编译为class文件
# 把class文件拷贝到target/classes目录,这个目录classes是存放类文件的根目录（也叫类路径，classpath）
# 插件：maven-compiler-plugin
# 插件：maven-resources-plugin 资源插件，把src/main/resources目录中的文件拷贝到target/classes目录中
mvn test-compile
# 编译命令，将src/test/java目录中的代码编译为class文件
# 把class文件拷贝到target/test-classes目录
# 把src/test/resources目录中的文件拷贝到target/taet-classes目录中
mvn test
# 测试命令，执行test-classes目录的程序，测试src/main/java目录中的主程序代码是否符合要求
# 插件：maven-surefire-plugin
mvn package
# 打包，把项目中的资源class文件和配置文件都放到一个压缩文件中
# 默认压缩文件是jar类型的，web应用是war类型，扩展是jar，war的
# 插件：maven-jar-plugin 执行打包处理，生成文件在target目录下
# 打包文件名：artifactId-version.packaging
# 打包文件包含/src/main目录中的所有的生成的class和配置文件，与test无关
mvn install
# 把生成的打包的文件安装到本地仓库
# 插件：maven-install-plugin
```
