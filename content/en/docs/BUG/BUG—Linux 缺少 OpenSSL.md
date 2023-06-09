---
title: Linux 缺少 OpenSSL
date: 2021-09-17
author: LMingJian
---

## BUG 描述

在 Linux 编译某些软件时，会出现报错：`fatal error: openssl/ssl.h: No such file or directory centos`。

## Resolution

这是缺少 OpenSSL 导致的，重新安装即可。

要在 Debian、Ubuntu 或者其他衍生版上安装 OpenSSL：

```shell
sudo apt-get install libssl-dev
```

要在 Fedora，CentOS 或者 RHEL 上安装 OpenSSL：

```shell
sudo yum install openssl-devel
```
