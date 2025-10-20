---
title: 如何配置 Linux 网卡自启
date: 2025-10-16T10:32:10+08:00
author: LiangMingJian
---

# 需求

最小安装时，Linux 网卡有可能没有被配置为开机自启，导致开机后网络没有连接，此时需要对网络配置以实现开机自启。

# 修改配置文件

**1.定位配置文件夹**

```bash
cd /etc/sysconfig/network-scripts/
```

**2.修改配置文件**

找到当前使用的网卡配置文件，如 `ifcfg-ens33`，将其中的 `ONBOOT` 配置改为 `yes`，设置为开机启动，保存并退出。

> 怎么找到当前网卡，可查阅：[ 如何固定 Linux 系统的 IP ](https://zhuanlan.zhihu.com/p/1961805481262683254)

**3.重启网卡或重启服务器以验证**

```bash
service network restart
reboot
```
