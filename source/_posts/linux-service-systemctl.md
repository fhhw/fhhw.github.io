---
title: Linux服务管理命令service与systemctl
date: 2021-07-24 12:46:03
tags: 
  - Service
  - Systemctl
categories: Linux
description: Linux 系统服务有时也称为守护程序，是在 Linux 启动时自动加载并在 Linux 退出时自动停止的系统任务。历史版本中的 linux 对服务的操作是通过 service 来完成的。若创建用户自定义的服务，则需要较为复杂的操作。目前 linux 新的发行版已经内置了 systemctl 来操作服务。
---

## 历史背景
Linux 系统服务有时也称为守护程序，是在Linux启动时自动加载并在Linux退出时自动停止的系统任务。历史版本中的linux对服务的操作是通过service来完成的。若创建用户自定义的服务，则需要较为复杂的操作。目前linux新的发行版已经内置了systemctl来操作服务。

在早期服务管理中，Init 是Linux系统启动时创建的第一个进程。它是一个守护进程，会一直运行到系统关闭。init 是其他所有进程的直接或间接祖先，并自动监护所有孤儿进程。内核按照硬编码的文件名启动它，如果内核不能启动它，将会导致内核崩溃。init 的进程标识符（PID）通常是 1。在系统启动和关闭时，init 进程会启动 init 脚本（或称 rc）来保障基本功能。这包括挂载和卸载文件系统，以及启动守护进程。进一步，有一个服务管理器提供对已启动进程的主动控制，称为进程监控。例如监测崩溃的进程并适时重启。这些元素加起来就成了 init 系统。某些 init 将服务管理器包含在 init 进程中，或是有紧密联系的 init 脚本。在下面，这类 init 将被称为整合式的。其他的分类下的条目可能会相互依赖。

目前，很多Linux发行版转向了systemd。systemd 是一个 Linux 系统基础组件的集合，提供了一个系统和服务管理器，运行为 PID 1 并负责启动其它程序。功能包括：支持并行化任务；同时采用 socket 式与 D-Bus 总线式激活服务；按需启动守护进程（daemon）；利用 Linux 的 cgroups (简体中文) 监视进程；支持快照和系统恢复；维护挂载点和自动挂载点；各服务间基于依赖关系进行精密控制。systemd 支持 SysV 和 LSB 初始脚本，可以替代 sysvinit。除此之外，功能还包括日志进程、控制基础系统配置，维护登陆用户列表以及系统账户、运行时目录和设置，可以运行容器和虚拟机，可以简单的管理网络配置、网络时间同步、日志转发和名称解析等。

Init系统是一系列简单精小服务构成的集合，配置起来很复杂，但是配置过程透明。而systemd则是一个包含很多功能的庞大系统，配置简单，对于这个转换，各方争论不一，有人喜欢它的功能强大，操作简单，有人批评它不符合Unix哲学。

## service命令
service是Init系统下进行服务管理的命令，service命令本身是一个shell脚本，它在/etc/init.d/目录查找指定的服务脚本，然后调用该服务脚本来完成任务。    
![Service](/img/linux/linux_service.png)

可通过sercice SCRIPT COMMAND命令管理服务，SCRIPT为/etc/init.d/中存放的可执行脚本文件：
```bash
service SCRIPT start    #启动服务
service SCRIPT stop     #停止服务
service SCRIPT restart  #重启服务
service SCRIPT status   #查看状态服务
```
此外还可以用service --status-all显示出所有系统服务列表，其中”+”代表服务正在运行，而”-“代表服务处于关闭状态，”?”代表根本没有状态这一说。  
查看运行服务还可以用 ps aux | grep service_name查看进程情况；如果是网络服务，还可以查看端口的监听情况，执行 netstat -tuln | grep service_name/port_number,例如可以执行 netstat -tuln | grep ftp查看端口状态，默认端口为21。  
用户可以添加自定义服务，将相应脚本放置于/etc/init.d/文件夹下。

## systemctl命令
### 服务管理
systemctl是systemd系统下的服务管理命令。它包含的功能很多，详见[systemctl](https://man.archlinux.org/man/systemctl.1)
```bash
systemctl status   #显示系统状态
systemctl start [单元]  #立即激活单元：
systemctl stop [单元]   #立即停止单元
systemctl restart [单元] #重启单元
systemctl enable [单元]  #开机自动激活单元
systemctl disable [单元] #取消开机自动激活单元
systemctl daemon-reload  #重新载入systemd，扫描新的或有变动的单元
```
Systemd使用“单元（Unit）”监控管理系统，单元文件是 ini 风格的纯文本文件。 封装了有关下列对象的信息： 服务(service)、套接字(socket)、设备(device)、挂载点(mount)、自动挂载点(automount)、 启动目标(target)、交换分区或交换文件(swap)、被监视的路径(path)、任务计划(timer)、 资源控制组(slice)、一组外部创建的进程(scope)。有关单元详细介绍参见[systemd.unit 中文手册](http://www.jinbuguo.com/systemd/systemd.unit.html)．

能用systemctl管理的服务需要有一个.service文件，在Ubuntu中，通常该文件位于/etc/systemd/system文件夹下，有关Systemd服务单元参见[systemd.service 中文手册](http://www.jinbuguo.com/systemd/systemd.service.html)．
![在这里插入图片描述](/img/linux/linux_systemctl.png)
当你更改了相关服务配置文件后，需要运行以下命令重启服务：

```bash
sudo systemctl daemon-reload && sudo systemctl restart name
# 其中name为你要重启的服务
```
### systemd 目标
Systemd中目标的概念，systemd 目标由目标单元表示。目标单元文件以 .target 文件扩展名结尾，它们的唯一用途是通过依赖项链将其他 systemd 单元分组在一起。例如，用于启动图形会话的 graphical.target 单元 将启动系统服务，如 GNOME 显示管理器 (gdm.service) 或帐户服务 (accounts-daemon.service)，还激活 multi-user.target 单元。同样，multi-user.target 单元会启动其他基本系统服务，如 NetworkManager (NetworkManager.service)或 D-Bus (dbus.service)，并激活另一个名为 basic.target 的目标单元。该模式替代了SysV init中预定义的运行级别。

**SysV 运行级别与 systemd 目标的比较**
| 运行级别 | 目标单元                            | 描述                         |
| -------- | ----------------------------------- | ---------------------------- |
| 0        | runlevel0.target, poweroff.target   | 关闭系统                     |
| 1        | runlevel1.target, rescue.target     | 设置救援 shell               |
| 2        | runlevel2.target, multi-user.target | 设置一个非图形化的多用户系统 |
| 3        | runlevel3.target, multi-user.target | 设置一个非图形化的多用户系统 |
| 4        | runlevel4.target, multi-user.target | 设置一个非图形化的多用户系统 |
| 5        | runlevel5.target, graphical.target  | 设置图形化多用户系统         |
| 6        | runlevel6.target, reboot.target     | 关闭并重启系统               |

命令：
```bash
# sysvinit 运行级别
runlevel   #查看当前运行级别
init N     #进入其它运行级别

# systemd 目标
systemctl list-units --type=target  #列出了当前加载的和激活的目标
systemctl isolate graphical.target  #切换目标
```
### 日志功能
此外，systemd提供了日志功能，查看日志是我们寻找问题的重要方法。journalctl 用来查询 systemd-journald 服务收集到的日志。systemd-journald 服务是 systemd init 系统提供的收集系统日志的服务`

日志功能常用命令如下：
```bash
journalctl            #不带任何选项时，默认输出所有的日志记录
journalctl -n [num]   #显示最后num行的日志，如果省略num,则默认显示最后10行
journalctl -f         #实时滚动显示最新日志
journalctl -u [unit]  #显示指定unit的日志
```
查看日志需要超级管理员权限。
```bash
# 具体用法参见
journalctl -h
# 例如查看docker服务最新日志
sudo journalctl -u docker | tail -n 50
```

## 参考文献
[systemd (简体中文)](https://wiki.archlinux.org/title/Systemd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
[使用 systemd 目标](https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/working-with-systemd-targets_configuring-basic-system-settings)
[linux journalctl 命令](https://www.cnblogs.com/sparkdev/p/8795141.html)
