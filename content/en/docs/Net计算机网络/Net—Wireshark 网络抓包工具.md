---
title: Wireshark 网络抓包工具
date: 2021-06-21
author: LM
---

## 1.Wireshark可以做什么

WireShark（前称Ethereal）是一个网络封包分析软件。网络封包分析软件的功能是抓取网络封包，并尽可能显示出最为详细的网络封包资料。WireShark 抓包是根据 TCP/IP 五层协议来的，也就是物理层、数据链路层、网络层、传输层、应用层，因此 Wireshark 可以做到以下事情：

- 网络管理员使用Wireshark检测网络问题
- 网安工程师用Wireshark检查信息安全相关问题
- 开发者使用Wireshark为新的通信协议调试
- 普通用户使用Wireshark学习网络协议相关知识

## 2.Wireshark不可以做什么

- Wireshark不是入侵侦测软件（Intrusion Detection Software, IDS）。对于网络上的异常流量行为，Wireshark不会产生警示或是任何提示。然而，仔细分析Wireshark截取的数据包能够帮助用户对于网络行为有更清楚的了解。
- Wireshark不会对网络数据包产生内容的修改 - 它只会反映出当前流通的数据包信息。 Wireshark本身也不会提交数据包至网络上。就是说你只能查看数据包，不能修改或转发。

## 3.抓包实例

打开wireshark，主界面如下：

![](/images/drawingbed/img/202204291751013.png)

双击上图的中的过滤器（捕获），开始抓包

![](/images/drawingbed/img/202204291751589.png)

打开 cmd 窗口，执行 ping 命令。工作中的 wireshark 将抓取到相关数据包，在过滤栏设置过滤条件以避免其他无用数据包影响分析，**比如：ip.addr == 185.199.111.153 and icmp 表示只显示ICPM协议且源主机IP或者目的主机IP为185.199.111.153的数据包。说明：协议名称icmp要小写**

![](/images/drawingbed/img/202204291751861.png)

## 4.数据包详细信息

![](/images/drawingbed/img/202204291751676.png)

- Frame: 物理层的数据帧概况 
- Ethernet II: 数据链路层以太网帧头部信息 
- Internet Protocol Version 4: 互联网层IP包头部信息 
- Transmission Control Protocol: 传输层T的数据段头部信息，此处是TCP
- Hypertext Transfer Protocol: 应用层的信息，此处是HTTP协议

## 5.TCP包的具体内容

从下图可以看到wireshark捕获到的TCP包中的每个字段。

![](/images/drawingbed/img/202204291752278.png)

## 6.抓包（捕获）过滤器语法和实例

抓包过滤器有如：类型Type（host、net、port）、方向Dir（src、dst）、协议Proto（ether、ip、tcp、udp、http、icmp、ftp等）、逻辑运算符（&& 与、|| 或、！非）等的分类

### （1）协议过滤

直接在过滤框中输入协议名即可。 tcp，只显示TCP协议的数据包列表 ；http，只查看HTTP协议的数据包列表 ；icmp，只显示ICMP协议的数据包列表

### （2）IP过滤

host 192.168.1.104 ；src host 192.168.1.104 ；dst host 192.168.1.104

### （3）端口过滤

port 80 ；src port 80 ；dst port 80

### （4）逻辑运算符&& 与、|| 或、！非

src host 192.168.1.104 && dst port 80 抓取主机地址为192.168.1.80、目的端口为80的数据包； host 192.168.1.104 || host 192.168.1.102 抓取主机为192.168.1.104或者192.168.1.102的数据包 ；! broadcast 不抓取广播数据包

## 7.显示过滤器语法和实例

### （1）比较操作符

比较操作符有 == 等于、! = 不等于、> 大于、< 小于、>= 大于等于、<= 小于等于。

### （2）协议过滤

直接在Filter框中输入协议名即可。注意：协议名称需要输入小写。 tcp，只显示TCP协议的数据包列表 ；http，只查看HTTP协议的数据包列表；icmp，只显示ICMP协议的数据包列表

### （3） ip过滤

ip.src =\=192.168.1.104 显示源地址为192.168.1.104的数据包列表； ip.dst==192.168.1.104, 显示目标地址为192.168.1.104的数据包列表； ip.addr == 192.168.1.104 显示源IP地址或目标IP地址为192.168.1.104的数据包列表

### （4）端口过滤

tcp.port ==80, 显示源主机或者目的主机端口为80的数据包列表。 tcp.srcport == 80, 只显示TCP协议的源主机端口为80的数据包列表。 tcp.dstport == 80，只显示TCP协议的目的主机端口为80的数据包列表。

### （5） Http模式过滤

http.request.method=="GET", 只显示HTTP GET方法的。

### （6）逻辑运算符为 and/or/not

过滤多个条件组合时，使用and/or。比如获取IP地址为192.168.1.104的ICMP数据包表达式为 ip.addr == 192.168.1.104 and icmp

### （7）按照数据包内容过滤。

假设我要以IMCP层中的内容进行过滤，可以单击选中界面中的码流，在下方进行选中数据。右键-->准备过滤器-->选中-->添加条件表达式，如`data contains "uestc"`

## 8.Wireshark分析TCP协议三次握手

### TCP三次握手连接建立过程

![](/images/drawingbed/img/202204291752094.png)

- 第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。
- 第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；
- 第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手，客户端与服务器开始传送数据。 {% endtabs %}

### wireshark抓取访问指定服务器数据包

1. 启动wireshark抓包，打开浏览器输入`www.cnblogs.com`
2. 终止抓包，输入 http 过滤
3. 隐藏其他无关http数据包，右键选中，追踪流——>TCP流，显示握手信息，如图所示：

![](/images/drawingbed/img/202204291752500.png)

可以看到这里截获了三个握手数据包，第四个是HTTP数据包，说明HTTP的确是使用TCP建立连接的。

------

**第一次**，客户端发送了一个TCP，标志位为SYN，序列号为0，表示客户端请求建立连接，如下：

![](/images/drawingbed/img/202204291752958.png)

**关键参数**

1. SYN ：标志位，表示请求建立连接
2. Seq = 0 ：初始建立连接值为0，数据包的相对序列号从0开始，表示当前还没有发送数据
3. Ack = 0：初始建立连接值为0，已经收到包的数量，表示当前没有接收到数据

------

**第二次**，服务器发回确认包, 标志位为 SYN,ACK. 将确认序号(Acknowledgement Number)设置为客户的ISN加1以.即0+1=1, 如下图如下：

![](/images/drawingbed/img/202204291753715.png)

**关键参数**

1. SYN + ACK：标志位，同意建立连接，并回送SYN+ACK

2. Seq = 0 ：初始建立值为0，表示当前还没有发送数据

3. Ack = 1：表示当前端成功接收的数据位数，虽然客户端没有发送任何有效数据，确认号还是被加1，因为包含SYN或FIN标志位。（并不会对有效数据的计数产生影响，因为含有SYN或FIN标志位的包并不携带有效数据）

------

**第三次**，客户端再次发送确认包(ACK) SYN标志位为0,ACK标志位为1.并且把服务器发来ACK的序号字段+1,放在确定字段中发送给对方.并且在数据段放写ISN的+1，如下图如下：

![](/images/drawingbed/img/202204291753661.png)

**关键参数**

1. ACK ：标志位，表示已经收到记录
2. Seq = 1 ：表示当前已经发送1个数据
3. Ack = 1 : 表示当前端成功接收的数据位数，虽然服务端没有发送任何有效数据，确认号还是被加1，因为包含SYN或FIN标志位（并不会对有效数据的计数产生影响，因为含有SYN或FIN标志位的包并不携带有效数据)。

{{< details "参考文件" >}} 
1：[ Wireshark抓包使用指南@Ju5tice ](https://zhuanlan.zhihu.com/p/82498482)
{{< /details >}}