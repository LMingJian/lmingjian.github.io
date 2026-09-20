---
title: 如何离线打包 yum 下载的安装包
date: 2026-05-09T11:47:16+08:00
author: LiangMingJian
---

# 需求

有一离线服务器，无法连接外网，但需要某些安装包。此时，如何通过另外一台可以连接外网的服务器下载安装包，然后复制到离线服务器，并进行安装。

# 使用 yumdownloader

`yumdownloader` 是 Yum 提供的一个工具，可以下载指定软件包及其依赖项。

所有下载的 RPM 包会保存在 `/root/mypackages` 目录中，通过复制到离线服务器进行安装。

```shell
# 安装 yum-utils 工具包
yum install -y yum-utils

# 下载软件包及其依赖项到指定目录
yumdownloader --resolve --destdir=/root/mypackages iperf3
```

# 安装下载的 rpm 包

将上述目录中的所有 rpm 包都复制到离线服务器中，通过执行以下命令，批量安装。

```shell
# 批量本地安装 rpm 包
yum localinstall *.rpm -y

# 忽略 GPG 签名的条件下批量安装 rpm 包
yum install *.rpm -y --nogpgcheck
```
