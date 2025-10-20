---
title: 如何配置 Linux 系统的 IP
date: 2024-12-25T13:55:12+08:00
author: LiangMingJian
---

# 需求

在**最小安装**的 Linux 或没有安装 GNOME 或其他可视化桌面环境时，如何仅通过命令行系统固定系统 IP。

# 修改网络配置文件

**1.定位配置文件夹**

```bash
cd /etc/sysconfig/network-scripts/
```

在这个文件夹中，存在着 Linux 系统的网络配置文件。输入 `ls` 指令，我们可以看见这里有多个文件，每一个文件都是一个网卡的配置文件。

![](_images/drawingbed/img/Pasted%20image%2020251015151737.png)

**2.找到配置文件并进行编辑**

一般情况下，第一个文件如 `ifcfg-eth0` 就是我们当前使用的网卡，我们可以通过以下指令进入文件进行编辑，从而固定设备 IP。

```bash
vi ifcfg-eth0
```

不过在实际运维时，不同的服务器网卡可能会有所不同，此时可以通过 `ip addr` 指令来查看当前使用的网卡名称。

![](_images/drawingbed/img/Pasted%20image%2020251015152408.png)

> lo，ifcfg-lo 是环回网卡，即 127.0.0.1，::1，localhost 的网卡。

**3.修改配置文件内容，固定设备 IP**

在进入 `ifcfg-eth0` 编辑模式后，按需求将文件内容修改为以下示例。

修改重点为：`BOOTPROTO, IPADDR, GATEWAY, PREFIX, DNS1, DNS2`。

```bash
# 网络类型
TYPE="Ethernet"
# 代理模式
PROXY_METHOD="none"
# 是否将代理用于浏览器
BROWSER_ONLY="no"
# 是否启动 DHCP：none 为禁用 DHCP；static 为使用静态 ip 地址
# 在固定 IP 时，必须使用 static
BOOTPROTO="static"
# 将该网卡作为默认路由出口
DEFROUTE="yes"
# IPv4 异常是否结束网络连接，yes 结束，no 不结束并尝试 IPv6
IPV4_FAILURE_FATAL="no"
# IPv6 相关配置
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
# 网卡名称
NAME="ens33"
# 网卡 ID
UUID="b4701c26-8ea8-46a5-b738-1d4d0ca5b5a9"
# 设备名称
DEVICE="ens33"  
# 自动启动
ONBOOT="yes"
# 当需要固定 IP 时，必须添加以下内容
# IP 地址
IPADDR=192.168.1.122 
# 子网掩码长度，24 即 255.255.255.0  
# 另外也可以使用 NETMASK=255.255.255.0 
PREFIX=24  
# 网关
GATEWAY=192.168.1.1  
# DNS
DNS1=223.5.5.5  
DNS2=223.6.6.6  
```

**4.重启网络服务**

```bash
service network restart
```

**5.重启系统以检查是否成功**

```bash
reboot
```
