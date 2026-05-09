---
title: 如何使用 Wireshark 进行网络抓包
date: 2024-12-25T14:00:27+08:00
author: LiangMingJian
---

# Wireshark 可以做什么

Wireshark 是一个网络数据包分析软件，它可以抓取网络数据包，并尽可能地列出这些网络数据包的资料。

需要注意的是，Wireshark 不是入侵侦测软件。它对网络上的异常流量行为，不会产生警示或是任何提示，它只会默默的记录。但用户可以通过分析 Wireshark 截取的数据包，来识别异常。

最后，Wireshark 不会对网络数据包产生内容的修改，它只会反映出当前流通的数据包信息。因此，你只能通过 Wireshark 查看数据包，不能修改或转发。

# 操作介绍

打开 Wireshark 软件，软件的主界面如下所示。当我们双击软件首页中的捕获（过滤器）时，Wireshark 便开始抓包了。

![](_images/drawingbed/img/Pasted%20image%2020260309163020.png)

在捕获界面中，主要包括控制栏，筛选栏，数据流，解包数据分析的这几个部分，对应的位置如下图所示。

![](_images/drawingbed/img/Pasted%20image%2020260309164033.png)

## 控制栏

从左到右，功能主要有开始，暂停，重新开始和设置。

![](_images/drawingbed/img/Pasted%20image%2020260309165816.png)

## 过滤器

用户可以通过多样的过滤语法对数据进行筛选。

![](_images/drawingbed/img/Pasted%20image%2020260309170013.png)

支持的过滤器语法包括：

- 协议过滤：如：tcp，http，icmp，特别的，支持 `http.request.method=="GET"`，只显示 HTTP GET 方法。
- IP 过滤：`ip.addr==, ip.src==, ip.dst==`，分别代表捕获指定地址，捕获源地址，捕获目标地址的数据包。
- 端口过滤：`port==, srcport==, dstport==`，分别代表捕获指定端口，捕获源主机端口，捕获目标主机端口的数据包。

基本的语法如上所示，结合逻辑运算符 `&&, ||, !, and, or, not`，用户可以组合上述语法捕获复杂内容。

特别的，如果用户需要针对某一条数据流的数据进行捕获，那么可以右键目标数据流，在菜单中选择`准备作为过滤器`来对数据包内容过滤。

![](_images/drawingbed/img/Pasted%20image%2020260309170717.png)

## 数据流

在 Wireshark 捕获界面的正中间，这部分内容就是捕获的数据流。

捕获的数据流会不断刷新，直到用户点击控制栏中的暂停。

![](_images/drawingbed/img/Pasted%20image%2020260309171015.png)

## 数据包分析

当用户点击数据流中任意一条数据时，Wireshark 捕获界面最底下的便会展示这条数据的十六进制源代码（右侧内容）以及数据分析结果（左侧内容）。

Wireshark 对数据包的分析是按照 OSI 七层网络模型进行分析的，数据分析结果（左侧内容）中的每一行都代表 OSI 七层网络模型中的其中一层内容。

- Frame：代表物理层，展示的是原始数据流概况。
- Ethernet II：代表数据链路层，展示的是以太网协议信息。
- 802.1Q Virtual LAN：这里的也是数据链路层，802.1Q 是 VLAN 相关协议，属于数据链路层的子层。
- Internet Protocol Version 4：代表网络层，展示的是 IP 协议信息。
- User Datagram Protocol：代表传输层，展示的是传输使用的协议信息，可以看见，这里的是 UDP 协议。
- Simple Service Discovery Protocol：这里应用层，展示应用层协议，此处是 SSDP 协议。

![](_images/drawingbed/img/Pasted%20image%2020260309173011.png)

# 实验：使用 Wireshark 分析 TCP 协议三次握手四次挥手

## 实验操作

第一步：启动 Wireshark，选择以太网，打开浏览器访问 `https://cn.bing.com`。

第二步，在 Bing 网页打开后，终止抓包，在过滤器中输入 `http` 筛选 HTTP 协议数据。

![](_images/drawingbed/img/Pasted%20image%2020260309175438.png)

第三步，我们右键选中第一条流，选择`追踪流`，`TCP 流`，追踪 TCP 握手信息。

![](_images/drawingbed/img/Pasted%20image%2020260309175634.png)

第四步，在追踪成功后，我们便可以看见完整的 TCP 握手信息。

![](_images/drawingbed/img/Pasted%20image%2020260309175926.png)

## 实验理论

众所周知，TCP 三次握手和四次挥手是 TCP 协议中用于建立和断开连接的两个核心机制。

TCP 三次握手用于建立连接，主要流程如下所示：

- 第一次握手，客户端向服务器发送一个 `SYN=1` 的报文，请求建立连接，进入 `SYN-SENT` 状态。
- 第二次握手，服务器在收到 SYN 后，会回复一个 `SYN=1, ACK=1` 的报文，同时包含自己的初始序列号，并确认客户端的序列号，进入 `SYN-RCVD` 状态。
- 第三次握手，客户端发送 `ACK=1` 报文，确认服务器的序列号，双方进入 `ESTABLISHED` 状态，连接正式建立，开始数据传输。

> SYN，Synchronize Sequence Numbers，同步序列编号。
> ACK，Acknowledgment Number，确认号。
> Seq，Sequence Number，序列号。

TCP 四次挥手用于断开连接，主要流程如下所示：

- 第一次挥手，主动关闭方（一般是客户端）发送 `FIN=1` 报文，表示数据发送完毕，进入 `FIN_WAIT_1` 状态。
- 第二次挥手，被动方（服务器）收到 FIN 信号后，发送 `ACK=1` 确认，进入 `CLOSE_WAIT` 状态，表示已收到关闭请求。
- 第三次挥手，服务器处理完剩余数据后，发送自己的 `FIN=1` 报文，进入 `LAST_ACK` 状态。
- 第四次挥手，客户端在收到 `FIN=1` 后，发送最终的 ACK 确认，进入 `TIME_WAIT` 状态，等待 2MSL 后客户端关闭，服务器在收到 ACK 后立即关闭连接。

> MSL，Maximum Segment Lifetime，最大报文生存时间，在 `TIME-WAIT` 状态下，为了确保最后的 ACK 报文能够到达和避免旧连接的混淆，客户端需要等待 2 个最大报文生存时间后才能关闭。

上述便是这次 TCP 三次握手，四次挥手的实验原理，现在我们对 Wireshark 捕获内容进行分析。

## 实验分析（三次握手）

首先看第一行数据，客户端（`192.168.2.117`）发送了一个 `SYN` 报文，序列号 Seq=0，这一步是 TCP 连接的第一次握手，客户端请求建立连接。

在这一步中，Seq = 0，表示数据包的相对序列号从 0 开始，当前还没有发送数据。同时在这一步骤，默认 Ack = 0，表示已经收到包的数量为 0，当前没有接收到数据。

![](_images/drawingbed/img/Pasted%20image%2020260309185807.png)

接着，我们看第二行数据，服务器（`183.47.118.249`）发送了一个 `[SYN, ACK]`，这是 TCP 连接的第二次握手，服务器进入等待连接的状态。

在这一步中，Seq = 0 表示服务器的数据包序列号也从 0 开始，Ack = 1 是因为服务器成功收到客户端的 SYN 信号，因此 Ack + 1。

![](_images/drawingbed/img/Pasted%20image%2020260309190651.png)

最后，我们看第三行数据，客户端在收到 `[SYN, ACK]` 后，便会发送一个 `ACK`，确认连接，完成第三次握手，开始传输数据。

在这一步骤，客户端的 Seq=1，Ack=1。

![](_images/drawingbed/img/Pasted%20image%2020260309191445.png)

在连接成功后，我们便看见正式 HTTP 协议请求（POST）了。

![](_images/drawingbed/img/Pasted%20image%2020260309191641.png)

————————————

## 实验分析（四次挥手）

我们看向倒数第四条数据（4897），这里服务器（`183.47.118.249`）作为主动关闭方，发送了一个 `FIN, ACK` 信号，告诉客户端这边要关闭连接了，进入 `FIN_WAIT_1` 状态。

![](_images/drawingbed/img/Pasted%20image%2020260309191942.png)

接着，客户端（`192.168.2.117`）在收到 `FIN` 信号后，发送一个 `ACK` 信号，确认关闭，进入 `CLOSE_WAIT` 状态。

![](_images/drawingbed/img/Pasted%20image%2020260309192054.png)

再来，客户端再发送一个 `FIN, ACK` 信号，告诉服务器我这边也准备好可以关闭了，进入 `LAST_ACK` 状态。

![](_images/drawingbed/img/Pasted%20image%2020260309192250.png)

最后，服务器发送 `ACK` 信号，并等待 2MSL 后，关闭自己的连接。客户端在收到服务器的 `ACK` 信号后，也关闭自己的连接。

![](_images/drawingbed/img/Pasted%20image%2020260309192359.png)

实验完成！

————————————

> 推荐阅读：[ 基于 Wireshark 抓包的 DHCP 协议分析 ](https://zhuanlan.zhihu.com/p/1977432678245610961)
