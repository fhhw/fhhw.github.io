---
title: 虚拟机安装 Arch Linux
date: 2022-11-24 15:51:59
tags: Linux
categories: Linux
description: 本文介绍如何在虚拟机中安装 Arch Linux。
---

## 背景

关于图形界面与命令行界面各有优势，在编译项目的场景下，更倾向于运行命令脚本自动化构建，但是在Windows系统中搭建Shell环境及其复杂，往往功能不全，很容易出现各种小问题。在Windows中也有很多模拟Linux环境的应用，最常见的有Cygwin，MSYS2，WSL等，我使用过其中的几个，使用体验不佳，普遍存在很多问题，而且极易造成系统混乱。那么问题来了，如何优雅的在Windows中使用Linux系统，我觉得最佳方案应该是部署Linux云服务器，这种方法成本比较高；那么另一种方案就是虚拟机了，但是虚拟机性能不高，为此，我选择了可以最小化安装的Arch Linux。

## 安装

### 软硬件准备

在虚拟机中新建一个客户机，硬件配置自行选择，网络适配器选择NAT模式，参见[虚拟机VMware Workstation Pro v16.0.0使用教程](https://zhuanlan.zhihu.com/p/447380486)，下载最新的[Arch Live ISO](https://archlinux.org/download/)，注意一定要选择最新的镜像，老版本镜像可能存在公钥过时问题。

### 安装步骤

相比于在真实主机中安装Linux系统，在虚拟机中安装要简单很多，不需要考虑网络连接、硬件驱动、兼容性等很多问题。

从镜像中启动系统，对于BIOS固件，Live ISO 使用syslinux作为引导程序；对于UEFI固件，Live ISO 使用GRUB作为引导程序，安装时不必过多在意这些。不同版本的镜像以及固件方式不同，引导界面显示内容不同，通常会有以下几下：

```bash
Arch Linux install medium (x86_64, UEFI)
Arch Linux install medium (x86_64, UEFI) with speech
EFI Shell
Reboot Into Firmware Interface
```

通常选择第一项即可，第二项提供了一个语音功能的无障碍安装模式，后面是一些进入固件的选项。

会以 root 用户进入 Live 系统：
![在这里插入图片描述](/img/linux/archlinux_1.png)

英语区与中文区通常不需要更改默认键盘布局(US)，确认固件类型：

```bash
ls /sys/firmware/efi/efivars
```

上述命令有输出内容则为UEFI启动，若文件夹不存在则为BIOS启动，虚拟机创建客户机时如果没有更改固件类型默认为BIOS模式。

检查网络连接：

```bash
ip link
ping archlinux.org
```

因为创建客户机时网络适配器选择的是NAT模式，此处网络连接应该会自动配置完成，即运行 ip link 输出中有IPv4地址，ping 命令测试可以连通。

接下来，对硬盘分区，使用 fdisk 或者 lsblk 列出可用硬盘：

```bash
fdisk -l
```

在虚拟机中上述命令输出结果通常为 /dev/sda，Live ISO 中提供了fdisk、gdisk、parted等分区工具，使用 fdisk 命令分区：

```bash
fdisk /dev/sda
```

根据命令提示完成分区操作，推荐分区如下：

**BIOS with** MBR

| 挂载点 | 分区                | 分区类型   | 建议大小     |
| ------ | ------------------- | ---------- | ------------ |
| [SWAP] | /dev/swap_partition | Linux swap | 512MiB       |
| /mnt   | /dev/root_partition | Linux      | 磁盘剩余空间 |

UEFI with GPT：

| 挂载点    | 分区                      | 分区类型              | 建议大小     |
| --------- | ------------------------- | --------------------- | ------------ |
| /mnt/boot | /dev/efi_system_partition | EFI system partition  | 512MiB       |
| [SWAP]    | /dev/swap_partition       | Linux swap            | 512MiB       |
| /mnt      | /dev/root_partition       | Linux x86-64 root (/) | 磁盘剩余空间 |

创建好分区之后，格式化分区：

```bash
mkfs.ext4 /dev/root_partition             # 根分区文件类型为ext4
mkswap /dev/swap_partition                # 交换分区
mkfs.fat -F 32 /dev/efi_system_partition  # EFI分区类型为FAT32
```

挂在分区：

```bash
mount /dev/root_partition /mnt            # 挂载根分区
mkdir /mnt/boot                           # 创建EFI分区挂载点
mount /dev/efi_system_partition /mnt/boot # 挂载EFI分区
swapon /dev/swap_partition                # 激活交换分区
```

接下来将系统安装到刚刚分区的硬盘中，使用 pacstrap 脚本将 Live ISO 中的文件复制到硬盘中：

```bash
pacstrap -K /mnt base linux vim
```

虚拟机中只需要最小化安装 base 和 linux 组件，不需要安装 linux-firmware，将 vim 安装进去便于编辑文件。

创建 fstab 文件：

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

进入新系统：

```bash
arch-chroot /mnt
```

设置时区，此处设置为国内时区：

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

语言建议默认选择英文。创建 hostname 与 hosts 文件：

```
/etc/hostname
myhostname

/etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        myhostname
```

更改镜像，国内使用 Arch 官方镜像速度很慢，改为国内镜像，编辑 /etc/pacman.d/mirrorlist 文件（建议更改系统文件时先备份再更改）：

```
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```

设置 root 密码，创建远程登陆用户（ssh禁止使用 root 用户远程登陆）：

```bash
passwd                 # 连续输入两次密码
groupadd group1        # 创建用户组 group1
useradd -m -g group1 -s /bin/bash test # 创建用户 test，加入 group1 组 
passwd test            # 为用户 test 创建密码
```

编辑 /etc/sudoers 文件，对创建的 test 用户赋予执行 sudo 的权限。

安装相关软件：

```bash
pacman -S dhcpcd         # DHCP 客户端
pacman -S openssh        # 远程登陆服务端
pacman -S open-vm-tools  # 开源实现的 VMTools
```

安装 CPU microcode：

```bash
pacman -S amd-ucode    # AMD CPU
pacman -S intel-ucode  # Intel CPU
```

安装启动引导程序，启动引导程序有很多，常见的有GRUB（支持 BIOS 与 UEFI）、rEFInd（支持 UEFI），选择 GRUB 作为启动引导程序：

BIOS 启动模式：

```bash
pacman -S grub 
grub-install --target=i386-pc /dev/sda 
grub-mkconfig -o /boot/grub/grub.cfg  # 生成 grub.cfg 文件
```

UEFI 启动模式：

```bash
pacman -S grub efibootmgr  # 需要安装 efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB 
grub-mkconfig -o /boot/grub/grub.cfg  # 生成 grub.cfg 文件
```

完成上述步骤后，重启系统：

```bash
exit          # 退出 arch-chroot
shutdown now  # 关机
```

关机后需要移除 ISO 镜像才能重新启动，进入系统后执行如下操作：

```bash
systemctl enable systemd-networkd systemd-resolved dhcpcd sshd  # 设置网络相关软件自启动
systemctl start systemd-networkd systemd-resolved dhcpcd sshd   # 启动网络相关软件
ip link             # 查看网卡
ip link set up eth1 # 激活网卡，eth1 为上述命令输出的网卡名
dhcpcd              # 运行 DHCP
```

经过上述操作，虚拟机应该可以正常连接到网络，如果不行，就重启一下虚拟机。然后查看虚拟机 IP 地址，从 Window 中用 ssh 协议登陆到虚拟机，这样在就可以实现复制粘贴功能，推荐一款开源的跨平台终端 [Tabby](https://tabby.sh/),主机与虚拟机的文件传输，可以使用 Tabby 终端附带的 SFTP 协议或者虚拟机的共享文件夹功能。
![在这里插入图片描述](/img/linux/archlinux_2.webp)

在配置完 C 运行环境之后，系统占用仅仅2.3G，相比于其他发行版本，真的精简很多，适合在虚拟机中运行，减少不必要的性能开销。

