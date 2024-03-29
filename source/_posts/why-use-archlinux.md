---
title: 为什么选择Arch Linux
date: 2021-12-18 17:16:20
tags: Linux
cover: /img/linux/why_use_archlinux_cover.png
categories: Linux
description: Arch Linux 是通用发行版，初始安装仅提供命令行环境：用户不需要删除大量不需要的软件包，而是可以从官方软件仓库成千上万的高质量软件包中进行选择，搭建自己的系统。
---

## 前言

之前因为要学习操作系统这门课程，打开了 Linux 的大门，折腾了很久，也算一个 Linux 入门选手。我是从 Ubuntu 桌面版开始接触 Linux 的，Ubuntu 桌面版集成了很多软件，入手简单易学，但不可避免显得很臃肿，虽然可以手动删除很多不需要的东西，但这个过程很麻烦，甚至于你无法把它删减成最简洁的系统，这对于有强迫症的我来说无法忍受。后来我转向 Arch，Arch 最大的特点是做加法，从安装开始，它一步步安装内核、固件、基础软件包，然后添加一些能用到的基本功能，从一开始，Arch 就遵循最简洁的原则，我需要的就是安装这样一个最简洁的系统。

使用 Arch Linux，你会学到很多关于 Linux 的知识，从安装开始，就是一个怎样一步步将系统跑起来的指南，分区、挂载、安装、配置……当你仔细阅读这些细节，一定会受益匪浅。给我的感觉是 Ubuntu 桌面版这种操作系统是从系统整体层面切入，而 Arch 是深入到包层面，目前我的学习阶段也只是深入到这个层面。当然，我不推荐使用基于Arch的 Manjaro，因为它失去了这种优势，变得和 Ubuntu 桌面版一样了。如果再深入学习，可以选择 Gentoo，从编译开始，可以说是深入源码层面了。

上面这个对比我只是从学习角度来比较的，稳定性暂时不考虑。以我的使用感受来说，Ubuntu 也有很多小问题，Arch 也没有出现过崩溃到需要重装系统的问题。出于好奇，我喜欢安装各种不同的图形界面，毕竟华丽的外表很有吸引力，但是图形界面有很多 Bug，这与 Linux 图形界面显示机制和硬件驱动有一定关系。

Arch 适合有一定 Linux 基础的人使用，定制化强就意味着很折腾，一直想记录下这个过程，搁置了很久，最近有时间了，打算记录一下，一方面以备以后查询，另一方面，给入手 Arch 的同学一个参考。

Linux 作为日常办公使用具有局限性，但在服务器上表现十分优秀。不可否认 Windows 也是一款优秀的桌面操作系统，我们日常使用的软件很多都只有 Windows 版本，而且日常使用的 Office 办公软件也都绑定在 Windows 上，尽管 Linux 也可以实现类似的功能，但实际操作过程与格式转换比较麻烦，所以我不打算把 Linux 系统作为日常办公系统。

学习使用 Linux，首先需要掌握一些基础知识，如常用命令、多用户模式、文件权限、常用软件使用等。Linux 系统上小问题可能比较多，遇到这些 bug，解决过程可能很复杂。另外养成随手保存和经常整理、备份资料的好习惯，不至于在系统出现故障时造成损失。

Linux 发行版=Linux 内核+包管理工具+桌面环境

![linux distributions](/img/linux/linux_distributions.png)

Linux 发行版

从表面来看，各个 Linux 发行版不同之处在于发行版集成的工具、包管理工具以及软件仓库的不同，而不是桌面环境（DE）的不同，当然还有稳定性、服务等方面的不同。

常见的 Linux 发行版有 Ubuntu、Fedora、openSUSE、Debian、Mint、CentOS、Arch、Gentoo、Deepin。

## Arch简介

Arch Linux 是通用 x86-64 GNU/Linux 发行版。Arch 采用滚动升级模式，尽全力提供最新的稳定版软件。初始安装的 Arch 只是一个基本系统，随后用户可以根据自己的喜好安装需要的软件并配置成符合自己理想的系统.

### **原则**

- **简洁**

避免任何不必要的添加、修改和复杂增加。它提供的软件都来自原始开发者(上游)，仅进行和发行版(下游)相关的最小修改。简单来说，就是我们一步步添加自己需要的功能，不必要的东西完全不用安装，做到了系统最小化。

- **现代**

Arch 尽全力保持软件处于最新的稳定版本，只要不出现系统软件包破损，都尽量用最新版本。Arch 采用滚动升级策略，安装之后可以持续升级。 Arch 向 GNU/Linux 用户提供了许多新特性，包括 systemd 初始化系统、现代的文件系统、LVM2/EVMS、软件磁盘阵列（软 RAID）、udev 支持、initcpio（附带 mkinitcpio）以及最新的内核。这个特点很激进，因为这种特性很容易造成不稳定，最新功能往往没有经过完整的测试，但也是这个功能，让你能用上最新的功能。很多在其他发行版中找不到的软件在Arch都能找到，只需一条命令就能安装。

- **通用**

Arch Linux 是通用发行版，初始安装仅提供命令行环境：用户不需要删除大量不需要的软件包，而是可以从官方软件仓库成千上万的高质量软件包中进行选择，搭建自己的系统。支持x86-64 架构。( 对 i686 架构的支持已经结束 ） Arch有一个易用的包管理系统Pacman，仅凭一条命令就升级整个系统。Arch还提供一个类似ports的包构建系统（Arch Build System），通过它可以轻松从源码构建和安装软件包，并用一个命令完成同步。你甚至可以用一个命令重新构建整个系统。Arch还提供Arch 用户仓库，它包含了成千上万个由用户维护的PKGBUILD脚本，配合makepkg工具，从编译到打包一气呵成。用户还能轻松构建和维护属于自己的自定义软件源。

## 参考资料

[Arch Wiki](http://link.zhihu.com/?target=https%3A//wiki.archlinux.org/title/Table_of_contents)
