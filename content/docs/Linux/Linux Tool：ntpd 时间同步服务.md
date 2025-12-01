---
title: Linux Tool：ntpd 时间同步服务
date: 2025-11-29T09:06:45+08:00
author: LiangMingJian
---

# 前言

在现实工作中，由于服务器的长时间运行，所以时间往往会出现偏差，此时进行时间同步是非常必要的。这里的时间同步通常会使用 ntp 服务器来完成。

NTP，Network Time Protocol，网络时间协议，是 TCP/IP 协议族里面的一个应用层协议，主要功能是用来使客户端和服务器之间进行时钟同步，提供高精准度的时间校正，以避免服务器在长期运行后的时间偏差。

**NTP 服务器在安装后，它既可以作为客户端去获取其他 NTP 服务器的时间，也可以作为服务端为其他客户端提供时间，不需要额外配置，当 NTP 作为服务端时，它会使用到 UDP 123 端口**。

# NTP 的安装

大部分 Linux 系统的 NTP 服务都是默认安装的，我们可以通过以下命令检查安装情况：

```bash
# 检查服务状态 
systemctl status ntpd 

# 检查同步状态 
ntpq -p 

# 检查同步信息 
ntpstat
```

如果发现 NTP 服务器没有安装，那么可以通过以下两种途径自行安装：

**通过 yum 软件库安装**

```bash
yum install -y ntp
```

**通过源代码离线安装**

下载 NTP 源代码文件：[ ntp-4.2.8p18.tar.gz ](https://downloads.nwtime.org/ntp/4.2.8/)，随后在服务器中，执行以下命令编译安装：

```bash
# 解压
tar -xvf ntp-4.2.8p18.tar.gz
# 进入文件夹
cd ntp-4.2.8p18
# 编译配置
./configure
# 编译安装
make && make install
```

> [ Building and Installing the Distribution ](https://www.ntp.org/documentation/4.2.8-series/build/)

**启动 NTP 服务**

```bash
systemctl start ntpd
systemctl enable ntpd
```

**开启防火墙**

```bash
firewall-cmd --zone=public --add-port=123/udp --permanent
firewall-cmd --reload
```

# NTP 的配置

NTP 只有唯一的配置文件 `/etc/ntp.conf`，该文件的配置内容主要有以下部分：

> 通过 `vi /etc/ntp.conf` 编辑配置文件

**权限控制关键字 restrict**

restrict 用于管理客户端访问权限，可以用来禁止客户端修改服务器时间，禁止状态查询，禁止远程日志等功能。

比如：默认配置中的 `restrict default nomodify notrap nopeer noquery` 就代表对所有未明确的客户端（`default`）禁止修改服务器时间（`nomodify`），禁止远程登录（`notrap`），禁止建立对等关系（`nopeer`），禁止查询服务器状态（`noquery`）。

而默认配置中的 `restrict 127.0.0.1` 和 `restrict ::1` 则代表允许本地所有权限。

另外如果想对内网特定网段同步时间，则可以使用配置项 `restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap`，允许 1 段访问。

![](_images/drawingbed/img/Pasted%20image%2020251129102840.png)

**时间服务器关键字 server**

server 用于设置同步使用的时间服务器，通常可以搭配 prefer，iburst 和 fudge 一起使用。prefer 表示优先使用，iburst 表示快速建立同步，这两者都能用来标记优先同步源。fudge 则用来设置服务器层级（值越小优先级越高）。

比如：配置项 `server ntp.aliyun.com prefer` 表示优先使用阿里 NTP 服务器同步时间，如果有多个 server，则按对应的优先级连接服务器同步时间。

而配置项 `fudge ntp.aliyun.com stratum 1` 则表示阿里 NTP 服务器所处优先级层级（stratum）为第一，数值越小优先级越高。

# 拓展阅读：如何使用硬件时钟作为同步时间源

在内网环境中，由于无法连接外网，因此没办法使用外部的同步时间源。

为了保证内网环境中所有的设备使用同一个时间，可以将一台服务器使用自身硬件时钟校准同步，然后将其作为其他设备的 NTP 服务器使用，以此实现局域网内的时间同步。

在 NTP 服务器中，**127.127.1.0** 是特殊的一个地址，它用来指代本地硬件时钟。因此，在配置文件中，我们可以将其视作外部时间源一样配置：

```bash
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

上述配置的含义为：使用本地时钟作为同步时间源，同时将优先级设置为第十。

![](_images/drawingbed/img/Pasted%20image%2020251129113127.png)

# 拓展阅读：配置正常但重启 NTP 服务失败

在重启 NTP 服务时，可能会出现配置正常，但重启失败的问题，此时需要用户检查端口是否被占用的问题。

NTP 使用 UDP 123 端口，当端口被占用时无法重启（即使占用端口的也是 NTP 服务，但由于某些原因，也可能无法被 systemctl 重启）。

此时需要通过以下命令，检查端口占用情况，然后杀掉进程，再重启服务。

```bash
# 占用检查
netstat -tunlp | grep 123

# 杀掉进程
kill pid

# 重启服务
systemctl restart ntpd
```
