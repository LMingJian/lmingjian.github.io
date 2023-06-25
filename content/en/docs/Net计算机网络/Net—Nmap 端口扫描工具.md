---
title: Nmap 端口扫描工具
date: 2020-11-17
author: LM
---

## 1.Nmap

**Nmap是一个网络连接端扫描软件，用来扫描网上电脑开放的网络连接端**。确定哪些服务运行在哪些连接端，并且推断计算机运行哪个操作系统（这是亦称 fingerprinting）。它是网络管理员必用的软件之一，以及用以评估网络系统安全。

[ Nmap中文网下载  ](http://www.nmap.com.cn/)

```bash
# 安装依赖
gcc -v # 需要c语言编译 
yum install gcc  # 安装编译器
yum install gcc-c++ # 安装需要c++
-----------------------------------------------------------
# 下载源码后解压，进入文件夹编译
tar jxvf Nmap.tar.bz2
cd Nmap.tar.bz2
./configure  
make && make install  # 安装
nmap ip  # 扫描端口
nmap -v  
# 如果出现nmap命令无法找到的错误
# 尝试进入nmap文件内，使用./nmap ip运行程序
```
