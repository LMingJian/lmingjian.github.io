---
title: 如何固定 Linux 系统的 IP
date: 2024-12-25T13:55:12+08:00
author: LiangMingJian
---

# 修改网络配置文件

在文件夹`/etc/sysconfig/network-scripts/`中，有着 Linux 系统的网络配置文件，其中`ifcfg-lo`是回环网卡，`ifcfg-ens33`就是`eth0`，通过编辑`eth0`网卡的配置文件可以固定系统 IP，执行命令`vim ifcfg-ens33`。

```bash
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
# 是否启动 DHCP：none 为禁用 DHCP；static 为使用静态 ip 地址
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="b4701c26-8ea8-46a5-b738-1d4d0ca5b5a9"
DEVICE="ens33"  
# 启动或者重启网络时是否启动该设备：yes是启用；no是禁用
ONBOOT="yes"

# 添加如下配置信息
DNS1=192.168.0.1          # DNS
IPADDR=192.168.1.122      # IP地址
GATEWAY=192.168.1.1       # 网关
PREFIX=24                 # centos子网掩码长度：24--> 255.255.255.0    

# 子网掩码 RedHat，不同版本的Linux的配置是不一样的 
# NETMASK=255.255.255.0 
```

重启网络服务`service network restart`，IP 固定成功

{{< details "参考文件" >}} 
1：[ Linux - 配置固定的ip地址 ](https://zhuanlan.zhihu.com/p/54512739)
{{< /details >}}
