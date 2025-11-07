---
title: Linux Tool：WonderShaper 网络限速
date: 2024-12-25T13:59:21+08:00
author: LiangMingJian
---

# 概述

某些时候，因为种种原因，我们不想要 Linux 跑满带宽，或者想要在 Linux 服务器网络质量较差的时候测试下东西，此时就需要对 Linux 进行限速。

WonderShaper 是用来对特定网卡进行快速限速的工具。它实际是对 Linux tc 命令进行封装后的 shell 脚本，所以使用成本比 tc 更低，更容易上手。

# 安装

```shell
# 直接拉取 WonderShaper，开箱即用
git clone https://github.com/magnific0/wondershaper.git

# 查看版本
cd wondershaper
./wondershaper -v
>>> Version 1.4.1
```

# 使用示例

**限制网卡速度（单位 Kbps）**

```bash
# 对网卡 ens33 进行限速， 以 Kbps 为单位
# 下行 200000 Kbps = 200 Mbit/s = 25 MB/s
# 上行 102400 Kbps = 100 Mbit/s = 12.5 MB/s
./wondershaper -a ens33 -d 200000 -u 100000
```

**取消限速**

```shell
./wondershaper -c -a ens33
```

**查看网卡状态**

```shell
./wondershaper -s -a ens33
```

# Ex.网速单位的互换

**带宽（Mbps）与下载速度（MB/s）**

- 1 Mbps = 0.125 MB/s
- 100 Mbps = 100/8 MB/s‌ = ‌12.5 MB/s‌

**比特（bit）与字节（Byte）**

- 1 Mbps = 1000 Kbps，1 Kbps = 1000 bps
- 1 MB/s = 1024 KB/s，1 KB/s = 1024 B/s

————————————

> [ 网卡限速工具之 WonderShaper  @知乎 ](https://zhuanlan.zhihu.com/p/559712246)
