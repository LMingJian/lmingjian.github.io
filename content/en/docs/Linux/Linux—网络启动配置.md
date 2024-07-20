---
title: Linux 网络启动与 SSH 开启
date: 2021-11-22
author: LM
---

## 1.开机启动网卡

在 Linux 安装后，有时候网卡可能没有设置自动启动，此时就需要修改文件夹`/etc/sysconfig/network-scripts/`中的内容来进行设置。

在文件夹`/etc/sysconfig/network-scripts/`中`ifcfg-lo`是回环网卡，`ifcfg-ens33`就是`eth0`。

编辑`ifcfg-ens33`，将`ONBOOT`改为`yes`，设置为开机启动，重启网卡。

```bash
vim /etc/sysconfig/network-scripts/ifcfg-ens33
service network restart
ping 114.114.114.114
```
## 2.网络配置查看

可以通过安装 net-tools，使用 ifconfig 命令查看所有的网络配置。

```bash
yum install net-tools
ifconfig
```

## 3.启动 SSH

```bash
service sshd start
```

