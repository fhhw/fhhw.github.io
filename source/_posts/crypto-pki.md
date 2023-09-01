---
title: PKI 简介
date: 2023-03-16 17:32:36
tags: PKI
categories: Cryptography
description: PKI 全称为 Public Key Infrastructure，中文翻译为公钥基础设施，是一组由硬件、软件、参与者、管理政策与流程组成的基础架构，其目的在于创造、管理、分配、使用、存储以及撤销数字证书。
---

## 什么是 PKI

PKI 全称为 Public Key Infrastructure，中文翻译为**公钥基础设施**，是一组由硬件、软件、参与者、管理政策与流程组成的基础架构，其目的在于创造、管理、分配、使用、存储以及撤销**数字证书**。

在现实世界中，你如何证明自己的身份呢？通常需要权威机构给你颁发一个实体的身份证明证件，这个证件包含着标识你的唯一 ID 和一些基本信息，可能还会包含着你的指纹和面部识别信息等生物特征。这样你可以通过这个证件来证明你的身份，验证你的身份，可以通过比对证件所记录的信息是否与你的生物特征匹配。

那么在网络世界如何标识一个个体的身份呢，除此之外，我们还需要安全的传输信息，由此诞生了 PKI。我们知道通过公私密钥对可以安全地传输消息，但是我们怎么获取要传输信息对象的公钥呢，PKI 将公钥与身份绑定在一起。

PKI 安全性最初出现在 1990 年代，旨在通过颁发和管理数字证书来帮助管理加密密钥。这些 PKI 证书验证私钥的所有者以及该关系的真实性，这些证书类似于数字世界的驾驶执照或护照。我们最常见的 PKI 包含在 HTTPS 协议中，通过 SSL 证书维护安全性，以便网站访问者知道他们正在将信息发送给预期的接收人。

## PKI 结构

![PKI architecture](/img/crypto/PKI_architecture.png)

如图所示，PKI 由以下几部分构成：

- PKI entity：使用 PKI 证书的最终用户。PKI 实体可以是操作员、组织、路由器或交换机等设备，也可以是计算机上运行的进程。PKI 实体使用 SCEP 与 CA 或 RA 通信。
- CA：授予和管理证书的证书颁发机构。CA 颁发证书、定义证书有效期并通过发布 CRL 吊销证书。
- RA：注册机构，通过处理证书注册请求来减轻 CA 工作负担。RA 接受证书请求，验证用户身份，并确定是否要求 CA 颁发证书。RA 在 PKI 系统中是可选的。如果存在将 CA 公开给直接网络访问的安全问题，建议将某些任务委托给 RA。然后，CA 可以专注于其签署证书和 CRL 的主要任务。
- Certificate/CRL repository：存储证书和 CRL（Certificate Revocation List） 并将这些证书和 CRL 分发到 PKI 实体的证书分发点。它还提供查询功能。PKI 存储库可以是使用 LDAP 或 HTTP 协议的目录服务器，其中通常使用 LDAP。

### 数字证书

PKI 通过颁发和管理数字证书来管理加密密钥。数字证书也称为 X.509 证书和 PKI 证书。数字证书绑定到加密密钥对，用于验证网站、个人、组织、用户、设备或服务器的身份。证书包含主体（即身份标识）以及数字签名。
数字证书具有以下特性： 

1. 是相当于驾驶执照或护照的电子文档；
2. 包含有关个人或实体的信息；
3. 由受信任的第三方颁发；
4. 防篡改；
5. 包含可以证明其真实性的信息；
6. 可以追溯到发行者；
7. 有一个到期日期；
8. 提供给某人（或某物）进行验证。

### 数字证书创建过程

![PKI-Certificate-Creation-Process](/img/crypto/PKI-Certificate-Creation-Process.png)
证书创建过程如下：

- 创建私钥并计算相应的公钥； 
- CA 请求私钥所有者的标识属性并审查该信息；
- 公钥和标识属性被编码到证书签名请求 （CSR） 中；
- CSR 由密钥所有者签名以证明拥有该私钥；
- CA 验证请求并使用自己的私钥对证书进行签名；

### CA 层级结构

![PKI-CA-Hierarchies-and-Root-CAs](/img/crypto/PKI-CA-Hierarchies-and-Root-CAs.png)
每个 CA 都有自己的证书，信任层通过 CA 层次结构创建，上层 CA 为下层 CA 颁发证书。存根证书是最初信任来源，根证书是自签名的，人们必须从本质上信任根证书颁发机构来信任可追溯到它的任何证书。为了满足最高的安全标准，根 CA 几乎不应处于联机状态。信任链如下图所示：

![Chain_of_trust_v2](/img/crypto/Chain_of_trust_v2.svg.png)

## 参考文献

[1]  [公开密钥基础建设 - 维基百科 ](https://zh.wikipedia.org/wiki/%E5%85%AC%E9%96%8B%E9%87%91%E9%91%B0%E5%9F%BA%E7%A4%8E%E5%BB%BA%E8%A8%AD)
[2]  [PKI architecture](https://techhub.hpe.com/eginfolib/networking/docs/switches/5130ei/5200-3946_security_cg/content/485048341.htm)
[3]  [What is PKI?](https://www.keyfactor.com/resources/what-is-pki/)
[4]  [Public key certificate - Wikipedia](https://en.wikipedia.org/wiki/Public_key_certificate)
