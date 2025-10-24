---
title: Linux 防火墙 firewall-cmd 操作指南
date: 2024-12-25T13:56:51+08:00
author: LiangMingJian
---

# 概述

firewall-cmd 是一个用于管理 Linux 防火墙的命令行工具。在大多数 Linux 发行版中，都采用 firewall-cmd 来进行防火墙控制。

一般情况下，firewall-cmd 都自带在系统中，如果没有安装，则可以使用以下命令：

```bash
# 安装防火墙
yum install firewalld

# 启动，设置开机自启，检查状态
systemctl start firewalld
systemctl enable firewalld
systemctl status firewalld

# 关闭
systemctl stop firewalld
```

> 防火墙是一种**网络安全系统**，其主要用于监控和控制进出网络的数据流量，通过预定义的安全规则允许或拒绝数据包传输，从而保护内部网络免受外部威胁和未经授权的访问。

# 基本使用

## 开放端口

```bash
firewall-cmd --zone=public --add-port=8080/tcp --permanent
# --zone=public：这组规则写在 public 安全域，public 是默认域
# --add-port=8080/tcp：添加 TCP 端口 8080
# --permanent：永久生效，若没有该参数，则系统重启后失效

firewall-cmd --zone=public --add-port=8080/udp --permanent
# --add-port=8080/udp：开放 UDP 端口

firewall-cmd --zone=public --add-port=10000-15000/tcp --permanent
# 批量打开从 10000 到 15000 之间的所有 TCP 端口
```

## 重载防火墙，使规则生效

```bash
# 重载防火墙，保持网络不中断（推荐使用）
firewall-cmd --reload

# 以下也是重载，但是网络会中断
firewall-cmd --completely-reload
```

## 删除端口

删除端口与开放端口的操作基本一致，只需将 `--add-port` 修改为 `--remove-port` 即可。

```bash
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
```

## 查看防火墙信息

```bash
# 查看开放端口
firewall-cmd --list-ports

# 查看设置的规则
firewall-cmd --list-all
```

# 高级操作

## 针对不同的网卡接口设置不同的规则

在服务器具有多个网卡时，其系统也会由多个网络接口，如 eth0，eth1 等等。此时，我们可以通过 firewall-cmd 的安全域 zone 来为不同的网络接口设置不同的防火墙规则。

```bash
# 查看系统默认设置的安全域
firewall-cmd --get-zones

# 查看当前生效的安全域
firewall-cmd --get-active-zones

# 查看指定接口所在的安全域
firewall-cmd --get-zone-of-interface=eth0

# 创建一个默认的安全域 Zone
firewall-cmd --permanent --new-zone=myzone

# 定义规则
firewall-cmd --zone=myzone --add-port=8080/tcp --permanent

# 将网络接口分配到指定安全域
firewall-cmd --zone=public --add-interface=eth0 --permanent 
firewall-cmd --zone=myzone --add-interface=eth1 --permanent 

# 重载防火墙使更改生效
firewall-cmd --reload
```

最后，firewalld 所有的安全域配置都可以在 `/etc/firewalld/zones` 中查看。

## 以服务进行管理

在 Linux 中，常用的一些服务如 SSH，FTP，HTTP 等都有自己的端口开放要求。正常来说，我们需要将这些端口一个一个开放，然后放通这些服务流量。

但在 firewalld 中，它提供一个快捷的方式，支持一键放通对应服务的数据流量。

```bash
# 查看当前所有支持的服务列表
firewall-cmd --get-services

# 查看当前某个服务所支持的规则
firewall-cmd --service=http --list-all

# 添加某个服务到安全域
firewall-cmd --zone=public --add-service=http --permanent

# 删除服务
firewall-cmd --zone=public --remove-service=http --permanent

# 重新加载配置
firewall-cmd --reload
```

## 端口转发

firewalld 支持将流向本机某个端口的流量重定向到其他端口或其他服务器的端口，实现负载均衡或端口隐藏的功能。

```bash
# 开启 IP 伪装功能
firewall-cmd --add-masquerade --permanent

# 将本机的 80 端口转发到 8080 端口
firewall-cmd --permanent --add-forward-port=port=80:proto=tcp:toport=8080

# 将本机的 80 端口转发到 192.168.1.200 的 8080 端口
firewall-cmd --permanent \
--add-forward-port=port=80:proto=tcp:toaddr=192.168.1.200:toport=8080

# 删除转发规则
firewall-cmd --permanent --remove-forward-port=port=80:proto=tcp:toport=8080

# 重新加载配置
firewall-cmd --reload

# 查看当前的转发规则
firewall-cmd --list-forward-ports
```
