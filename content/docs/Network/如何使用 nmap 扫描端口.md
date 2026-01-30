---
title: 如何使用 nmap 扫描端口
date: 2024-12-25T14:00:17+08:00
author: LiangMingJian
---

# 概述

Nmap 是当前最通用的一种开源网络安全扫描工具，其支持主机发现，端口扫描，服务识别在内的多种功能，在评估系统安全时提供强大助力。

Nmap 通过扫描服务器的所有端口，检查那些端口允许访问，根据开放的端口的特征，确定有哪些服务运行在服务器上运行，进而推断服务器的漏洞。

# 简单使用

访问官网：[ Nmap ](https://nmap.org/)，点击 Download 下载对应平台的安装包，然后进行安装。

![](_images/drawingbed/img/Pasted%20image%2020260130163415.png)

在安装完成后，可以双击 Nmap 软件进行可视化操作，也可以直接通过 Shell 命令来使用。

> 使用 shell 时，应当将安装文件夹添加到系统环境遍历 path 中，默认安装地址：C:\Program Files (x86)\Nmap

```shell
# T4 设置扫描时序，-A 全部扫描，-v 详细输出
nmap -T4 -A -v 192.168.1.1
```

![](_images/drawingbed/img/Pasted%20image%2020260130164716.png)

# 从源代码安装

当某些系统不支持 RPM 安装时，建议直接通过源代码编译安装。

可以从 Github 仓库 [ https://github.com/nmap/nmap ](https://github.com/nmap/nmap) 或者 [ Nmap ](https://nmap.org/) 官网进行下载。

官网需要在 Download 页定位到：

![](_images/drawingbed/img/Pasted%20image%2020260130163903.png)

![](_images/drawingbed/img/Pasted%20image%2020260130163917.png)

详细的安装步骤可以参考下面的指令：

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

# 附录 1：基本参数

**扫描方式**

- `-sS`：TCP SYN 扫描（半开放扫描，不完全进行 TCP 连接）
- `-sT`：TCP 扫描
- `-sU`：UDP 扫描
- `-sA`：TCP ACK 扫描
- `-sW`：TCP Window 扫描
- `-sM`：TCP Maimon 扫描
- `-sN`：TCP Null 扫描
- `-sF`：TCP FIN 扫描
- `-sX`：TCP Xmas 扫描
- `-sY`：SCTP INIT 扫描
- `-sZ`：SCTP COOKIE-ECHO 扫描
- `-sO`：IP Protocol 扫描

**扫描内容**

- `-sP`：Ping 扫描，仅检查主机是否存活
- `-sn`：不进行端口扫描，仅发现主机
- `-sL`：列表扫描，仅列出目标主机
- `-sV`：检测服务版本
- `-O`：检测操作系统类型
- `-A`：启用全部扫描

**端口扫描参数**

- `-p <port>`：扫描指定端口
- `-p-`：扫描所有端口（1-65535）
- `-p 1-1000`：扫描指定端口范围
- `-F`：快速扫描模式（扫描前 1000 个端口）
- `-r`：按顺序扫描端口，不随机化
- `--top-ports <number>`：扫描最常见的端口

**输出参数**

- `-oN <file>`：输出为普通文本格式
- `-oX <file>`：输出为 XML 格式
- `-oG <file>`：输出为 Grepable 格式
- `-oS <file>`：输出为 Script Kiddie 格式
- `-oA <basename>`：同时输出三种主要格式（.nmap, .xml, .gnmap）

**性能参数**

- `-T0` 到 `-T5`：设置扫描时序，级别越高扫描越快但越容易被防火墙检测
- `--host-timeout <time>`：设置主机超时时间
- `--scan-delay <time>`：设置扫描延迟
- `--max-rate <number>`：设置最大包速率

**其他参数**

- `-6`：启用 IPv6 扫描
- `-e <iface>`：指定使用的网络接口
- `-f`：使用分片技术发送数据包
- `-v`：详细输出
- `--data-length <num>`：添加随机数据到数据包
- `--spoof-mac <mac>`：设置模拟 MAC 地址

# 附录 2：Windows 系统下 Nmap 使用 GUI 界面会报编码异常的解决方案

下载最新 Nmap 7.98 可避免大部分编码异常问题。

此外，应当尽量避免当前 Windows 用户是中文名。
