---
title: Linux Tool：WonderShaper
date: 2024-12-25T13:59:21+08:00
author: LiangMingJian
---

# 概述

WonderShaper 是用来对特定网卡进行快速限速的工具，它实际是对 linux 的 tc 命令进行封装后的 shell 脚本，所以使用成本比 tc 更低，更容易上手。

# 安装

```shell
# 直接拉取 WonderShaper，开箱即用
git clone https://github.com/magnific0/wondershaper.git

# 查看版本
./wondershaper -v
Version 1.4.1
```

# 帮助手册

```shell
./wondershaper -h
USAGE: ./wondershaper [-hcs] [-a <adapter>] [-d <rate>] [-u <rate>]

Limit the bandwidth of an adapter

OPTIONS:
   -h           Show this message 【帮助信息】
   -a <adapter> Set the adapter  【指定网卡接口】
   -d <rate>    Set maximum download rate (in Kbps) and/or 【限制下载速度(Kbps)】
   -u <rate>    Set maximum upload rate (in Kbps)   【限制上传速度(Kbps)】
   -p           Use presets in "/etc/systemd/wondershaper.conf"
   -f <file>    Use alternative preset file
   -c           Clear the limits from adapter 【清除指定网卡规则，用于取消限速】
   -s           Show the current status of adapter 【显示当前网卡的状态】
   -v           Show the current version  【显示当前版本】

   Configure HIPRIODST in "/etc/systemd/wondershaper.conf" for hosts
   requiring high priority i.e. in case ssh uses dport 443.

MODES:
   wondershaper -a <adapter> -d <rate> -u <rate>
   wondershaper -c -a <adapter>
   wondershaper -s -a <adapter>

EXAMPLES: 【使用示例】
   wondershaper -a eth0 -d 1024 -u 512  【设置网卡eth0的上行速度为512kbps，下行速度为1024kbps】
   wondershaper -a eth0 -u 512 【只设置上行速度为512kbps】
   wondershaper -c -a eth0 【清除网卡eth0的规则】
   wondershaper -p -f foo.conf 【设置指定的配置文件】

```

# 示例

## 限制网卡速度（单位 Kbps）

```shell
# 下行 2048kbps = 2 Mbit/s，上行 1024kbps = 1 Mbit/s
./wondershaper -a eno1 -d 2048 -u 1024
```

## 取消限速

```shell
# 取消限速
./wondershaper -c -a eno1

# 查看网卡状态
./wondershaper -s -a eno1
qdisc fq_codel 0: root refcnt 2 limit 10240p flows 1024 quantum 1514 target 5.0ms interval 100.0ms memory_limit 32Mb ecn
Sent 123022 bytes 471 pkt (dropped 0, overlimits 0 requeues 0) 
backlog 0b 0p requeues 0
maxpacket 0 drop_overlimit 0 new_flow_count 0 ecn_mark 0
new_flows_len 0 old_flows_len 0

```

# Ex.网速单位转换

```
1KB/s = 8kbps = 8kb/s
比如一般 100M 的宽带，实际是 100Mbps=(100/8) MB/s=12.5 MB/s
```

{{< details "参考文件" >}} 
1：[ 网卡限速工具之 WonderShaper  @知乎 ](https://zhuanlan.zhihu.com/p/559712246)
{{< /details >}}
