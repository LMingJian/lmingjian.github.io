---
title: 如何使用 Wireshark 进行网络抓包
date: 2024-12-25T14:00:27+08:00
author: LiangMingJian
---

# Wireshark 可以做什么

Wireshark（前称Ethereal）是一个网络封包分析软件。网络封包分析软件的功能是抓取网络封包，并尽可能显示出最为详细的网络封包资料。Wireshark 抓包是根据 TCP/IP 五层协议来的，也就是物理层、数据链路层、网络层、传输层、应用层，因此 Wireshark 可以做到以下事情：

- 网络管理员使用 Wireshark 检测网络问题
- 网安工程师用 Wireshark 检查信息安全相关问题
- 开发者使用 Wireshark 为新的通信协议调试
- 普通用户使用 Wireshark 学习网络协议相关知识

# Wireshark 不可以做什么

- Wireshark 不是入侵侦测软件（Intrusion Detection Software, IDS）。对于网络上的异常流量行为，Wireshark 不会产生警示或是任何提示。然而，仔细分析 Wireshark 截取的数据包能够帮助用户对于网络行为有更清楚的了解。
- Wireshark 不会对网络数据包产生内容的修改 - 它只会反映出当前流通的数据包信息。 Wireshark 本身也不会提交数据包至网络上。就是说你只能查看数据包，不能修改或转发。

# 示例

打开 Wireshark，主界面如下：

![](/_images/drawingbed/img/202204291751013.png)

双击上图的中的过滤器（捕获），开始抓包

![](/_images/drawingbed/img/202204291751589.png)

打开 cmd 窗口，执行 ping 命令。工作中的 Wireshark 将抓取到相关数据包，在过滤栏设置过滤条件以避免其他无用数据包影响分析，**比如：ip.addr == 185.199.111.153 and icmp 表示只显示ICPM协议且源主机IP或者目的主机IP为185.199.111.153的数据包。说明：协议名称icmp要小写**

![](/_images/drawingbed/img/202204291751861.png)

# 数据包解析

![](/_images/drawingbed/img/202204291751676.png)

- Frame: 物理层的数据帧概况 
- Ethernet II: 数据链路层以太网帧头部信息 
- Internet Protocol Version 4: 互联网层IP包头部信息 
- Transmission Control Protocol: 传输层T的数据段头部信息，此处是TCP
- Hypertext Transfer Protocol: 应用层的信息，此处是HTTP协议

# TCP 包的具体内容

从下图可以看到 Wireshark 捕获到的 TCP 包中的每个字段。

![](/_images/drawingbed/img/202204291752278.png)

# 支持的过滤器语法

## （1）协议过滤

直接在过滤框中输入协议名即可。如：tcp，只显示 TCP 协议的数据包列表 ；http，只查看HTTP协议的数据包列表 ；icmp，只显示ICMP协议的数据包列表；

特别的，支持`http.request.method=="GET"`, 只显示 HTTP GET 方法。

## （2）IP过滤

- `ip.addr==192.168.1.104 `，显示源地址或目标地址为 192.168.1.104 的数据包列表。
- `ip.src==192.168.1.104` ，显示源地址为 192.168.1.104 的数据包列表。
- `ip.dst==192.168.1.104`，显示目标地址为 192.168.1.104 的数据包列表。

## （3）端口过滤

- `port==80` ，显示源主机或者目的主机端口为 80 的数据包列表。
- `srcport==80` ，显示源主机端口为 80 的数据包列表。
- `dstport==80`，显示目的主机端口为 80 的数据包列表。

## （4）逻辑运算符 &&、||、!

- `src host 192.168.1.104 && dst port 80`，抓取主机地址为 192.168.1.80、目的端口为 80 的数据包。
- `host 192.168.1.104 || host 192.168.1.102`，抓取主机为 192.168.1.104 或者 192.168.1.102 的数据包。
- `! broadcast` ，不抓取广播数据包。
- 也支持使用`and`，`or`，`not`等关键字

## （5）按照数据包内容过滤。

可以单击选中界面中的码流，在下方进行选中数据，然后右键-->准备过滤器-->选中-->添加条件表达式，进行数据包内容过滤

# 示例：使用 Wireshark 分析 TCP 协议

1. 启动 Wireshark 抓包，打开浏览器输入`www.cnblogs.com`。
2. 终止抓包，输入 http 过滤， 隐藏其他无关 http 数据包。
3.右键选中，追踪流——>TCP 流，显示握手信息，如图所示：

![](/_images/drawingbed/img/202204291752500.png)

可以看到这里截获了三个握手数据包，第四个是 HTTP 数据包，说明 HTTP 的确是使用 TCP 建立连接的。

## 第一次握手

客户端发送了一个 TCP，标志位为 SYN，序列号为 0，表示客户端请求建立连接，如下：

![](/_images/drawingbed/img/202204291752958.png)

**关键参数：**

1. SYN ：标志位，表示请求建立连接。
2. Seq = 0 ：初始建立连接值为 0，数据包的相对序列号从 0 开始，表示当前还没有发送数据。
3. Ack = 0：初始建立连接值为 0，已经收到包的数量，表示当前没有接收到数据。

## 第二次握手

服务器发回确认包, 标志位为 SYN，ACK，将确认序号（Acknowledgement Number）设置为客户的 ISN 加 1，如下图如下：

![](/_images/drawingbed/img/202204291753715.png)

**关键参数**

1. SYN + ACK：标志位，同意建立连接，并回送 SYN+ACK
2. Seq = 0 ：初始建立值为 0，表示当前还没有发送数据
3. Ack = 1：表示当前端成功接收的数据位数，虽然客户端没有发送任何有效数据，确认号还是被加1，因为包含 SYN 或 FIN 标志位。（并不会对有效数据的计数产生影响，因为含有 SYN 或 FIN 标志位的包并不携带有效数据）

## 第三次握手

客户端再次发送确认包 ACK，SYN 标志位为 0，ACK 标志位为 1，并且把服务器发来 ACK 的序号字段 +1，放在确定字段中发送给对方，并且在数据段放写 ISN 的+1，如下图如下：

![](/_images/drawingbed/img/202204291753661.png)

**关键参数**

1. ACK ：标志位，表示已经收到记录
2. Seq = 1 ：表示当前已经发送1个数据
3. Ack = 1 : 表示当前端成功接收的数据位数，虽然服务端没有发送任何有效数据，确认号还是被加 1，因为包含 SYN 或 FIN 标志位（并不会对有效数据的计数产生影响，因为含有 SYN 或 FIN 标志位的包并不携带有效数据)。

# Ex.TCP 三次握手连接建立过程

![](/_images/drawingbed/img/202204291752094.png)

- 第一次握手：建立连接时，客户端发送 syn 包（syn=j）到服务器，并进入 SYN_SENT 状态，等待服务器确认。（SYN：同步序列编号（Synchronize Sequence Numbers））
- 第二次握手：服务器收到 syn 包，必须确认客户的 SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即 SYN+ACK 包，此时服务器进入 SYN_RECV 状态。
- 第三次握手：客户端收到服务器的 SYN+ACK 包，向服务器发送确认包 ACK（ack=k+1），此包发送完毕，客户端和服务器进入 ESTABLISHED（TCP连接成功）状态，完成三次握手，客户端与服务器开始传送数据。

{{< details "参考文件" >}} 
1：[ Wireshark抓包使用指南@Ju5tice ](https://zhuanlan.zhihu.com/p/82498482)
{{< /details >}}
