---
title: Ubuntu 18 中安装 GCC 编译器
date: 2020-07-14 21:55:03
tags: 
  - Ubuntu
  - GCC
categories: Linux
cover: /img/linux/ubuntu-painted.webp
description: GCC 是 GUN Compiler Collection 的简称，是由 GNU 开发的编程语言编译器。本文介绍了在 Ubuntu 18 中安装 GCC 的方法。
---

该文章是小编刚开始接触Linux的第一篇文章，那个时候连cd、ls…… 这些基本命令还没搞明白，现在回看，不免有些不当之处，现予以修改。

# 软件安装模式
Linux中安装软件一般有两种方式：
 1. 源码包，源码包是在本机上完成编译安装，根据具体硬件编译，相对于直接安装二进制包性能上有一定的提升，但安装过程比较繁琐，需要手动输入命令编译，初学者实操难度比较高。
 2. 二进制包，在Ubuntu中为.deb包，为编译好了的程序，安装过程简单。
 ```bash
 # 下载的.deb包可以通过dpkg命令安装
 sudo dpkg -i install-example-version.deb
 ```

 # 包管理软件
Linux操作系统都提供了一种中心化的机制用来搜索和安装软件。软件通常都是存放在存储库中，并通过包的形式进行分发。处理包的工作被称为包管理。包提供了操作系统的基本组件，以及共享的库、应用程序、服务和文档。包管理系统除了安装软件外，它还提供了工具来更新已经安装的包。包存储库有助于确保你的系统中使用的代码是经过审查的，并且软件的安装版本已经得到了开发人员和包维护人员的认可。

包管理软件自动处理软件之间的依赖关系，在Ubuntu系统中，包管理命令是apt，它集合了传统的apt-get 和 apt-cache等命令的大部分功能。


# GCC简介
通常所说的GCC是GUN Compiler Collection的简称，是由GNU开发的编程语言编译器。GNU编译器套件包括C、C++、 Objective-C、 Fortran、Java、Ada和Go语言前端，也包括了这些语言的库。GCC的初衷是为GNU操作系统专门编写的一款编译器。GNU系统是彻底的自由软件。大家可以进入[GCC官网](http://gcc.gnu.org/)查看相关信息。

# 安装步骤

 1. 在终端输入

```bash
sudo apt install build-essential
```
build-essential是一整套工具，执行完后，就完成了gcc,g++,make的安装。

 2. 检查是否安装成功

```bash
gcc -v
```
若出现gcc版本号，则证明安装成功
# 常见问题
这里列举一个我在安装过程中遇到的问题，在执行完sudo apt install build-essential后，出现了:

```bash
hemu@ubuntu:~$ sudo apt install build-essential
...
The following packages have unmet dependencies:
 build-essential : Depends: libc6-dev but it is not going to be installed or
                            libc-dev
                   Depends: g++ (>= 4:4.4.3) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```
为什么会出现这个问题呢，当时小编也很懵，明明按照教程一步一步做的，而我却出现了bug。现在就来探究一下问题的根源，由于众所周知的原因，国内访问Ubuntu官方软件源很慢，所以需要更换软件源，小编就百度了一下“Ubuntu更换软件源”，也没多想就随便找了一篇教程把软件源粘贴上去了，结果，小编用的是Ubuntu18.04，软件源对应“bionic”仓库，却粘贴成了Ubuntu16.04的“xenial”仓库，可想而知，软件版本之间发生了冲突，经过小编这么长时间的探索发现，手动安装软件很多时候bug都是由软件版本之间不匹配引起的。

关于Ubuntu版本的命名规则和开发代号，Ubuntu版本的命名规则是根据正式版发布的年月命名，Ubuntu18.04LTS 也就意味着 2018年4月发布的 Ubuntu，有LTS的为长期维护版本。Ubuntu 的开发代号有三个特点：
1. 都是动物
2. 都是两个词，并且两个词的首字母相同
3. 从6.06开始，首字母从D开始递增

比如Ubuntu18.04LTS的开发代号为Bionic Beaver，所以其官方软件源为：
deb http://cn.archive.ubuntu.com/ubuntu/ $\underline{bionic}$ main restricted universe multiverse

接下来我们需要将其更换为国内软件源，/etc/apt/sources.list文件即为系统软件源列表，我们首先将其备份。
```bash
cd /etc/apt
sudo cp sources.list sources.list.bak
sudo vim sources.list
```
然后从[阿里巴巴开源镜像站 - 阿里云开发者社区](https://developer.aliyun.com/mirror/)中寻找相对应的软件源，例如Ubuntu18.04LTS为：

```bash
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

```
然后更新一下软件：

```bash
sudo apt-get update
sudo apt-get upgrade
```
然后在安装gcc。
