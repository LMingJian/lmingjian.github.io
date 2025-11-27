---
title: 基于 Wireshark 抓包的 DHCP 协议分析
date: 2025-11-27T10:43:55+08:00
author: LiangMingJian
---

# 实验目的

DHCP，Dynamic Host Configuration Protocol，动态主机配置协议，是一种用于动态分配 IP 地址和其他网络参数的协议。用户端设备可以 DHCP 协议自动配置包括 IP 地址、子网掩码、网关在内的网络参数，从而简化网络管理和维护工作。

下文将通过抓包软件 Wireshark 来捕获设备网络接口上的 DHCP 数据包，并通过对数据包的分析来对 DHCP 协议的构成和行为逻辑进行详细介绍，以求更好的理解。

# 实验理论

## DHCP 协议的相关概念

- **DHCP 的名称**：动态主机配置协议（Dynamic Host Configuration Protocol）。
- **DHCP 的作用**：自动为网络中的主机配置包括 IP 地址、子网掩码、网关等网络参数。
- **DHCP 的地址池**：DHCP 自维护的一个 IP 地址区间，所有为主机分配的 IP 地址都从这个区间中提取，当这个地址池耗尽时，DHCP 将不再分配 IP 地址。
- **DHCP 的端口**：DHCP 是 C/S 架构，服务端使用 67 端口，客户端使用 68 端口。
- **DHCP 的传输**：无连接，高效的 UDP 协议。

## DHCP 协议的原理

![](_images/drawingbed/img/Pasted%20image%2020251127111539.png)

DHCP 是 C/S 架构，在同一个网络中，可以有多个 DHCP 服务器。如上图所示，客户端在通过 DHCP 协议获取 IP 地址时，主要需要进行以下四步：

**步骤一**

客户端会在网络中**广播 DHCP Discovery**。

这个数据包会携带客户端的 MAC 地址，当这个数据包到达 DHCP 服务器时，DHCP 服务器会响应，并进行下一步。

**步骤二**

服务器会在网络中**广播 DHCP Offer**。

所有在网络中收到 DHCP Discovery 广播的 DHCP 服务器都会给发出广播的客户端返回一个 DHCP Offer，这个数据包携带这个 DHCP 可以使用的 IP 地址，但不会携带其他参数，其作用是告诉客户端你可以使用这个 IP 地址。

**步骤三**

客户端在网络中**广播 DHCP Request**。

客户端在收到 DHCP Offer 后，会立即发送 DHCP Request，告诉 DHCP 服务器我要用这个 IP 地址，请把详细参数发送给我。

> 注意，客户端对 DHCP Offer 的接收逻辑是先到先用，即多个 DHCP 服务器发送的 DHCP Offer，哪一个服务器的先被客户端收到，客户端就使用那个 IP，其他的都会被忽略。

**步骤四**

服务器会在网络中**广播 DHCP Ack**

服务器在收到 DHCP Request 后，就会确认这个请求所要的 IP 地址在不在自己的地址池中，有没有被占用。如果一切正常，则会给客户端发送 DHCP Ack，表示确认请求，告诉客户端你可以使用这个 IP 地址了，详细参数如子网掩码、DNS、网关、租期等内容也会随这个数据包一起发送。

——————

以上便是 DHCP 协议的主要流程。

特别的，DHCP 协议所发放的 IP 地址一般是携带租期的，这个租期的作用是告诉客户端，你可以在这个租期时间端内使用这个 IP 地址。

因此，客户端在租期结束后需要发送一个 **DHCP Release** 来告诉 DHCP 服务器这个 IP 地址我不再使用了。不过大部分情况下，客户端会在租期截至前重新发送 **DHCP Request** 来让 DHCP 服务器更新当前 IP 地址的租期，完成续约。

## DHCP 协议的报文类型

![](_images/drawingbed/img/Pasted%20image%2020251127133719.png)

# 实验过程

**步骤一**

下载并安装网络封包分析软件 Wireshark，下载地址 [ Download Wireshark ](https://www.wireshark.org/download.html)

![](_images/drawingbed/img/Pasted%20image%2020251127161849.png)

**步骤二**

启动 Wireshark，并**双击**选择需要捕获的网络接口。

（这里选择捕获以太网接口）

![](_images/drawingbed/img/Pasted%20image%2020251127162025.png)

**注意**：通过在终端执行 `Get-NetAdapter -IncludeHidden`，可以查看各网络接口名称

![](_images/drawingbed/img/Pasted%20image%2020251127162154.png)

**注意**：Adapter for loopback traffic capture 这个接口是本地环回网络接口，即 IP 地址 127.0.0.1 的网络接口，流经这个地址的数据会在这里捕获。

**步骤三**

在过滤栏中输入 `dhcp`，筛选 DHCP 协议流量。

![](_images/drawingbed/img/Pasted%20image%2020251127162740.png)

**步骤四**

分别执行 `ipconfig /release` 和 `ipconfig /renew`，释放当前的动态 IP 地址，然后重新获取一个新的动态 IP 地址。

**步骤五**

检查捕获内容，开始进行分析。

![](_images/drawingbed/img/Pasted%20image%2020251127163057.png)

# 实验分析

显然，从上面步骤五的实验结果中，不难发现，DHCP 协议的实际使用与理论一致。从 DHCP Release 释放 IP 开始，先通过广播 DHCP Discovery，让 DHCP 服务器返回 DHCP Offer，再广播确认 DHCP Request，最后通过 DHCP Ack 结束。

DHCP Discovery 与 DHCP Request 的发送源地址（Source）与发送目的地（Destination）分别是 0.0.0.0 和 255.255.255.255，这表示的就是全网络广播。

DHCP Release，DHCP Offer，DHCP Ack 都有明确的发送源地址（Source），这发送源地址就是 DHCP 服务器地址，一般这个服务器会是路由器。而发送目的地（Destination）则是 255.255.255.255，这同样也是表示是广播。

点击 DHCP Discovery 这个数据，我们可以在软件下面的板块中查看对应的数据包情况。

![](_images/drawingbed/img/Pasted%20image%2020251127164525.png)

一条完整的数据包含以下五个内容：

**Frame 帧数据**

这里包含数据包在物理层被捕获时的原始信息以及捕获过程的元数据，这里的数据并不代表任何协议内容，只是 Wireshark 特有的包装信息。

我们可以简单，快速的从 Frame 数据中获取包括捕获字节数‌，捕获时间，捕获协议等内容。

> 关注：`Arrival Time`，`Capture Length`，`[Protocols in frame]`

![](_images/drawingbed/img/Pasted%20image%2020251127170804.png)

**Ethernet II（Ethernet V2）以太网**

从这里开始就是实际网络传输中的数据包内容，这一行内容相当于头信息。

Ethernet II 以太网是当今局域网采用的最通用的通信协议标准，Ethernet II 这个标头表示数据包是按 Ethernet II 协议格式进行编写并传输的，我们可以通过这行内容识别数据的发送目的地，发送源地址，以及使用的是 IPv4 还是 IPv6。

> 关注：`Destination`，`Source`，`Type`

![](_images/drawingbed/img/Pasted%20image%2020251127171700.png)

**Internet Protocol Version 4（IPv4）**

这里表示数据包使用的是 IPv4 协议，我们也可以从这里查看发送目的地，发送源地址等信息，此外还可以看到下一层使用到的协议。

> 关注：`Protocol`，`Source Address`，`Destination Address`

![](_images/drawingbed/img/Pasted%20image%2020251127172805.png)

**User Datagram Protocol（UDP）**

这里表示使用到 UDP 协议，我们可以从这里看到 UDP协议所链接的端口信息。

> 关注：`Source Port`，`Destination Port`

![](_images/drawingbed/img/Pasted%20image%2020251127173433.png)

**Dynamic Host Configuration Protocol (Discover)**

这里表示的就是 DHCP 协议的 Discovery 请求。展开这个请求，我们不难看出其通过 Option 携带的各种网络信息。

![](_images/drawingbed/img/Pasted%20image%2020251127173702.png)

# 总结

通过 Wireshark 的抓包分析，DHCP 协议的运行逻辑直观的展示在用户屏幕中。当然，除了 DHCP 协议，我们常说的 TCP 三次握手和四次挥手，HTTP，HTTPS 这些内容都可以通过 Wireshark 进行抓包分析。

另外，在网络异常时，通过 Wireshark 也可以帮助用户分析问题来源。比如 IP 地址分配异常时，就可以通过 Wireshark 抓包 DHCP 协议，来看看是哪一个 IP 的 DHCP 服务器发送了错误的 IP 地址，还可以通过对这个 DHCP 服务器进行端口扫描，来确定设备位置。
