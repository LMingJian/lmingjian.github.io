---
title: Linux Tool：mtr 网络链路测试
date: 2024-12-25T13:58:59+08:00
author: LiangMingJian
---

# 概述

当客户端在使用 ping 命令测试网络时，如果目标出现丢包或不通，那么我们可以通过 MTR 工具对网络链路进行测试，判断问题来源。

MTR（My traceroute）是大部分 Linux 发行版本预装的网络测试工具，它将 **ping** 和 **traceroute** 命令合并到同一个工具中。

相对于 traceroute 命令只会做一次链路跟踪测试，MTR 命令会对链路上的相关节点做持续探测并给出相应的统计信息，因此，MTR 命令能避免节点波动对测试结果的影响。

> Windows 中类似的链路测试工具称为：`tracert`，其使用基本于 `traceroute` 一致。

# 使用

```shell
mtr www.bing.com
```

![](_images/drawingbed/img/Pasted%20image%2020251107114731.png)

默认配置下，返回结果中各数据列的说明如下。

- 第一列（Host）：节点 IP 地址和域名。
- 第二列（Loss%）：节点丢包率。
- 第三列（Snt）：每秒发送数据包数。默认值 10，可以通过参数 -c 指定。
- 第四列（Last）：最近一次的探测延迟值。
- 第五、六、七列（Avg、Best、Wrst）：分别是探测延迟的平均值、最小值和最大值。
- 第八列（StDev）：标准偏差，越大说明相应节点越不稳定。

MTR 工具在进行网络链路测试时，默认发送  ICMP 数据包进行链路探测。但是在某些时候，目标服务器或者中间节点可能会配置防火墙禁止 ICMP 数据包进入。此时，我们可以通过指定 `-u` 参数，强制使用 UDP 数据包进行探测。

```shell
mtr www.bing.com -u
```

同样的，我们也可以使用 `-T` 参数，指定使用 TCP 数据包进行探测。

```shell
mtr www.bing.com -T
```
