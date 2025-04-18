---
title: Linux Tool：mtr
date: 2024-12-25T13:58:59+08:00
author: LiangMingJian
---

# 概述

当客户端访问目标服务器或负载均衡，使用 ping 命令测试出现丢包或不通时，可以通过 MTR 等工具进行链路测试来判断问题来源。

- MTR（My traceroute）是几乎所有 Linux 发行版本预装的网络测试工具，此工具也有对应的 Windows 版本，名称为 WinMTR。
- MTR 工具将 **ping** 和 **traceroute** 命令的功能并入了同一个工具中，实现更强大的功能。
- Linux 版本的 MTR 命令**默认发送 ICMP 数据包**进行链路探测。可以**通过 -u 参数来指定使用 UDP 数据包**用于探测。
- 相对于 traceroute 命令只会做一次链路跟踪测试，MTR 命令会对链路上的相关节点做持续探测并给出相应的统计信息。所以，MTR 命令能避免节点波动对测试结果的影响。

# 使用

```
mtr 192.168.1.243
```

默认配置下，返回结果中各数据列的说明如下。

- 第一列（Host）：节点 IP 地址和域名。如前面所示，按 n 键可以切换显示。
- 第二列（Loss%）：节点丢包率。
- 第三列（Snt）：每秒发送数据包数。默认值是10，可以通过参数 -c 指定。
- 第四列（Last）：最近一次的探测延迟值。
- 第五、六、七列（Avg、Best、Wrst）：分别是探测延迟的平均值、最小值和最大值。
- 第八列（StDev）：标准偏差。越大说明相应节点越不稳定。
