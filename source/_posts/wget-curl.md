---
title: 终端下载工具wget与curl
date: 2021-07-20 10:45:08
tags: Linux
categories: Linux
cover: /img/linux/wget_curl.png
description: Wget 工具与 Curl 工具都用来与服务器之间传输数据，有时候也用来测试网络连接。通常 Wget 多用于下载文件，Curl 多用于测试网络的连通性。
---

Wget工具与Curl工具都用来与服务器之间传输数据，有时候也用来测试网络连接。通常Wget多用于下载文件，Curl多用于测试网络的连通性。

# Wget命令
## 概述
GNU Wget 是一个免费实用程序，用于从 Web 非交互式下载文件。它支持HTTP、HTTPS和FTP协议，以及通过HTTP代理检索。下载地址为[http://www.gnu.org/software/wget/](http://www.gnu.org/software/wget/)  

Wget 有如下特性：  
+ Wget 是非交互式的，这意味着它可以在后台工作，而用户未登录。这允许你开始检索并断开与系统的连接，让 Wget 完成工作。相比之下，大多数 Web 浏览器都需要用户始终在场，这在传输大量数据时会是一个很大的障碍。  
+ wget 可以跟踪HTML页面上的链接依次下载来创建远程服务器的本地版本，完全重建原始站点的目录结构。这又常被称作”递归下载”。  
+ wget 非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性.如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕。如果是服务器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用。  
  

详细内容参见[Wget手册](http://www.gnu.org/software/wget/manual/)
## 调用
```bash
wget [option]… [URL]…
```
### 网络格式
URL是统一资源定位器的首字母缩写词。统一资源定位符是通过 Internet 提供的资源的紧凑字符串表示。Wget根据RFC1738识别URL语法 。这是最广泛使用的形式（方括号表示可选部分）：
```bash
http://host[:port]/directory/file
ftp://host[:port]/directory/file
```
还可以在 URL 中对用户名和密码进行编码：
```bash
ftp://user:password@host/path
http://user:password@host/path
```
无论用户或密码，或两者，可以被排除在外。如果省略HTTP用户名或密码，则不会发送任何身份验证。如果省略FTP用户名，'匿名的' 将会被使用。如果省略FTP密码，你的电子邮件地址将作为默认密码提供。  

### 常见用法
```bash
wget -O filename URL           # 保存文件为 filename
wget -c URL                    # 断点续传
wget -c -r -np -nc -L -p URL   # 递归下载文件
```

### 常用选项
长选项所必须的参数在使用短选项时也是必须的。  
启动：
```bash
-h,  --help              #打印此帮助。
-b,  --background        #启动后转入后台。
```
日志和输入文件：
```bash
-d,  --debug               #打印大量调试信息
-q,  --quiet               #安静模式 (无信息输出)
-v,  --verbose             #详尽的输出 (此为默认值)
-nv, --no-verbose          #关闭详尽输出，但不进入安静模式
```
下载：
```bash
-t,  --tries=NUMBER            #设置重试次数为 NUMBER (0 代表无限制)
     --retry-connrefused       #即使拒绝连接也是重试
-O,  --output-document=FILE    #将文档写入 FILE
-nc, --no-clobber              #不要重复下载已存在的文件
-c,  --continue                #断点续传下载文件
     --no-proxy                #禁止使用代理
```
目录：
```bash
-nd, --no-directories           #不创建目录
-x,  --force-directories        #强制创建目录
-nH, --no-host-directories      #不要创建主目录
-P,  --directory-prefix=PREFIX  #以 PREFIX/... 保存文件
     --cut-dirs=NUMBER          #忽略远程目录中 NUMBER 个目录层
```
# Curl工具
## 简介
curl是一个非常强大的工具，它用来与服务器之间传输数据，其支持的协议包括 (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, TELNET and TFTP)，curl设计为无用户图形界面交互下完成工作。  
curl提供了一大堆非常有用的功能，包括代理访问、用户认证、ftp上传下载、HTTP POST、SSL连接、cookie支持、断点续传。  
curl可用于多种平台，不仅仅是Linux平台，其官方网址为[https://curl.se](https://curl.se)  
## 安装
以Ubuntu20.04为例，可以采取源码包安装方式，下载对应平台源码压缩包，解压后放在合适位置，然后添加环境变量；为简单起见，可通过apt命令安装二进制包：
```bash
sudo apt install curl
```
## curl用法详解
### 命令格式
```bash
curl [options] [URLs]
```


### 常见用法
1、显示网页
```bash
curl https://curl.se
```
执行该命令，https://curl.se 页面以html格式显示在屏幕上，由于安装linux的时候很多时候是没有安装桌面的，也意味着没有浏览器，因此这个方法也经常用于测试一台服务器是否可以到达一个网站，此外，常用于测试终端代理是否成功。  
2、下载文件
```bash
curl https://curl.se >> curl.html  #使用linux的重定向功能保存
curl -o curl.html https://curl.se  #使用curl的内置option:-o(小写)保存文件
curl -O https://curl.se  #使用curl的内置option:-O(大写)保存文件，这里的url要具体到某个文件，不然下载不下来
```
3、下载github文件
下载github仓库里一个具体文件方法如下，首先找到源文件链接：
![github raw download](/img/linux/github_raw_download.png)
如上图所示，点击raw找到源文件链接，然后按照上述方式下载文件。

### 其他参数
curl命令参数非常多，此处只介绍常用的几项即可：  
```bash
-C/--continue-at <offset>             #断点续转
-f/--fail                             #连接失败时不显示http错误
-o/--output                           #把输出写到该文件中
-O/--remote-name                      #把输出写到该文件中，保留远程文件的文件名
-s/--silent                           #静音模式，不输出任何东西
-T/--upload-file <file>               #上传文件
-u/--user <user[:password]>           #设置服务器的用户和密码
-x/--proxy <host[:port]>              #在给定的端口上使用HTTP代理
-#/--progress-bar                     #进度条显示当前的传送状态
```
其他参数可以参考[How to Use](https://curl.se/docs/manpage.html)文档。  
# Httpie工具
curl与wget诞生于很早之前，虽然如此，它们依旧功能强大。现在也有很多有意思的新项目，比如HTTPie（读作 —aitch-tee-tee-pie—）——一个命令行 HTTP 客户端，拥有直观的界面，支持 JSON、语法高亮、下载功能（类似 wget）、插件支持等特性，其GitHub项目地址为[https://github.com/httpie/httpie](https://github.com/httpie/httpie)．

其设计目标是让 CLI 和 Web Service 的交互变得更友好。它提供了一个简单的 http 命令，可以让我们用简单且自然的语法发送任意 HTTP 请求，并且可以输出具有高亮显示的请求结果。HTTPie 可用于测试、调试以及通用的与 HTTP 服务器进行交互的场景。

apt软件源描述如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e37dc8e21ba4420887d75d115facc3bf.png)
HTTPie 详细介绍参见[HTTPie 中文文档](https://httpie.cn/doc/)．
