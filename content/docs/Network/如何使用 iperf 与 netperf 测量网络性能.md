---
title: 如何使用 iperf 与 netperf 测量网络性能
date: 2024-12-25T14:00:12+08:00
author: LiangMingJian
---

# iperf 

iperf 是一种网络性能测试工具，能对协议、定时、缓冲区等参数进行配置调整，能够测试 **TCP/UDP 最大带宽、延迟抖动、数据包丢失**等信息。

**iperf 基于 Server/Client 的工作模式**，客户端向服务端发送一定数量的数据，服务端统计并记录带宽、延时抖动等信息。客户端将数据全部发送后，服务端会回复一个数据包给客户端，将测试数据反馈给客户端。不过，如果网络较为拥塞或者误码率较高，客户端无法收到服务端回复的数据包，则只能显示本地记录的部分测试结果，所以服务端和客户端的测试结果可能有所不同。

[ iperf 官网下载 ](https://iperf.fr/)

```bash
# 安装
yum install iperf3
rpm -ivh xxxx.rpm
# iPerf命令语法格式
iperf [-s|-c host] [options]
iperf3 -s # 服务器
iperf3 -c ServerIP # 客户机
# 配置软链接
cd /usr/bin
ln -s iperf3 iperf
```

从源代码进行交叉编译，生成可执行文件。

```bash
tar -zxvf iperf-3.1.3.tar.gz        # 解压
cd iperf-3.1.3/                     # 进入解压目录
mkdir arm_install_dir               # 创建安装目录

./configure --host=arm-linux-gnueabihf --prefix=/home/dong/WorkSpace/Program/iperf-3.1.3/arm_install_dir/ CFLAGS=-static
# --host设置使用的编译器；   --prefix 安装目录； CFLAGS静态编译

make clean                          # 清除掉之前编译的文件，确保不影响
make                                # 编译
make install                        # 安装
```

# netperf 

Netperf 是一种网络性能测量工具，主要用于**测试 TCP 或 UDP 和 Berkeley 套接字接口的批量数据传输（bulk data transfer）和请求/应答（request/reponse）性能**。

**Netperf 工具以 Client/Server 方式工作**，服务端是 netServer，用来侦听来自客户端的连接，客户端是 netperf，用来向服务发起网络测试。在客户端与服务端之间，首先建立一个控制连接，传递有关测试配置的信息，以及测试的结果。在控制连接建立并传递了测试配置信息以后，客户端与服务端之间会再建立一个测试连接，用于来回传递特殊的流量，以测试网络的性能。当 netServer 在服务端启动后，就可在客户端运行 netperf 来测试网络的性能。

[ netperf 官网下载  ](https://hewlettpackard.github.io/netperf/)

```bash
rpm -ivh xxxx.rpm
netperf -V
# 安装完以后，会生成两个工具：netserver 和 netperf
netserver -D -p 9991 # 服务器，-D 前台运行，-p 指定端口
netperf -H host # 客户端
```

# 网络性能的指标

- **网络吞吐量**：单位时间内通过某个网络（信道或接口）的数据量，吞吐量受网络的带宽或者网络的额定速率限制，单位通常表示为bit/s或bps。
- **网络延时**：一个数据包从用户的计算机发送到网站服务器，然后再立即从网站服务器返回用户计算机的来回时间。影响网络延时的主要因素是路由的跳数和网络的流量。交换机延时（Latency）是指从交换机接收到数据包到开始向目的端口复制数据包之间的时间间隔。有许多因素会影响交换机延时大小，比如转发技术等等。
- **抖动**：用于描述包在网络中的传输延时的变化，抖动越小，说明网络质量越稳定越好。抖动是评价一个网络性能的最重要的因素。
- **丢包率**：理想状态下是发送了多少数据包就能接收到多少数据包，但是由于信号衰减、网络质量等诸多因素的影响并不能达到理想状态，而丢包率就是指测试中所丢失的数据包数量占所发送的数据包的比率。

# 基于 iPerf 测试 SDN 网络

## TCP 测试

步骤1：在主机1上执行 **iperf -s** 命令，以主机1为服务器端进行TCP测试。

```bash
---------------------------------
Server listening on 5201
-----------------------------------
# 说明：服务器端默认端口为 5201
```

步骤2：在主机2上执行 **iperf -c 主机1-IP** 命令，以主机2为客户端去连接主机1，测试主机1与主机2之间的吞吐量。

```bash
iperf -c 192.168.1.243
----------------------------------------------
[ID] Interval         Transfer      Bandwidth
[4] 0.00-30.00  sec   341 MBytes     95.4 MBytes/sec
# 结果表明主机1与主机2之间的带宽是95.4 Mbit/s
```

步骤3：**添加参数**测试以下命令

```bash
iperf -c 主机1-IP -t 32 -i 8
# -t 32 表示测试时间为32s，-i 8 表示输出频率为8s。该命令表示每8s输出一次测试结果，直到达到32s为止
iperf -c 主机1-IP -n 2000M -i 5
# -n 2000M 表示传输的数据量为2000M，-i 5 表示输出频率为5s。该命令表示每5s输出一次测试结果，到最接近总时间为止，最后再输出总的测试结果
```

## UDP 测试

步骤1：在主机1上执行 **iperf -s** 命令，以主机1为服务器端进行UDP测试。

```bash
---------------------------------
Server listening on 5201
-----------------------------------
# 说明：服务器端默认端口为5201
```

步骤2：在主机2上执行命令 **iperf -c 主机1-IP -u**，以主机2为客户端去连接主机1，测试主机1与主机2之间的网络性能。

```bash
iperf -c 192.168.1.243
----------------------------------------------
[ID] Interval         Transfer      Bandwidth          Jitter     Lost/Total Datagrams
[4] 0.00-30.00  sec   341 MBytes     95.4 MBytes/sec    0.421 ms    70050/77741 (90%)
# 结果表明主机1与主机2之间的带宽是95.4 Mbit/s
```

步骤3：**添加参数**测试以下命令

```bash
iperf -c 主机1-IP -u -b 2000M -i 5
# -b 2000M 指定客户端以2000Mbps为数据发送速率，-i 5 表示输出频率为5s
```

# 基于 Netperf 测试 SDN 网络

## TCP 测试

步骤1：在主机1上运行服务器端，用-p指定监听端口为9991

```bash
netserver -D -p 9991
```

步骤2：在主机2上运行客户端，指定服务器端的IP地址以及端口。缺省情况下Netperf进行TCP批量传输，即-t TCP_STREAM

```bash
netperf -H 192.168.1.180 -p 9991
MIGRATED TCP STREAM TEST from 0.0.0.0 (0.0.0.0) port 0 AF_INET to 192.168.1.180 (0.0.0.0) port 0 AF_INET
Recv   Send    Send                          
Socket Socket  Message  Elapsed              
Size   Size    Size     Time     Throughput  
bytes  bytes   bytes    secs.    10^6bits/sec  

 87380  16384  16384    10.47      86.40   
# 服务器端使用87380字节大小的socket接收缓冲，客户端使用16384字节大小的socket发送缓冲。缺省情况下，Netperf向发送的测试分组大小设置为本地系统所使用的socket发送缓冲大小，即向服务器端发送的测试分组大小也是16384字节，用时10.47s，吞吐量为86.4*10^6 bits/s。
1 Byte = 8 bit 
1 KB= 1024 B 
1 MB = 1024 KB 
1 GB = 1024 MB 
1 TB = 1024 GB
```

## UDP 测试

步骤3：在主机2上执行以下命令进行UDP测试

```bash
netperf -t UDP_STREAM -H 10.0.0.2 -p 9991
netperf -t UDP_STREAM -H 192.168.180.130 -l 60
netperf -t UDP_STREAM -H 192.168.180.130  -- -m 2048
# UDP_STREAM: UDP批量传输
# -l 指定时间 -m 设置本地系统发送测试分组的大小
```

步骤4：尝试使用以下命令

```bash
netperf -t TCP_RR -H 172.20.35.40 -l 10 -- -r 256,2048
# -r 用于指定客户端和服务端每次的交互数据量，上面表示客户端每次发送256字节，服务器每次回复2048字节
# TCP_RR 长连接 TCP_CRR 短连接 UDP_RR 长连接 
```
