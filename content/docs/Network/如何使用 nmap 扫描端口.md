---
title: 如何使用 nmap 扫描端口
date: 2024-12-25T14:00:17+08:00
author: LiangMingJian
---

# 概述

**Nmap 是一个网络连接端扫描软件，用来扫描网上电脑开放的网络连接端**。确定哪些服务运行在哪些连接端，并且推断计算机运行哪个操作系统（这是亦称 fingerprinting）。它是网络管理员必用的软件之一，以及用以评估网络系统安全。

[ Nmap ](https://nmap.org/)

```bash
# 安装依赖
gcc -v # 需要 c 语言编译 
yum install gcc  # 安装编译器
yum install gcc-c++ # 安装需要 c++
-----------------------------------------------------------
# 下载源码后解压，进入文件夹编译
tar jxvf Nmap.tar.bz2
cd Nmap.tar.bz2
./configure  
make && make install  # 安装
nmap ip  # 扫描端口
nmap -v  
# 如果出现 nmap 命令无法找到的错误
# 尝试进入 nmap 文件内，使用 ./nmap ip 运行程序
```
