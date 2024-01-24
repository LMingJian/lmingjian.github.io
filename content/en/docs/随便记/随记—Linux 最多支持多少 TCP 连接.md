---
title: Linux 最多支持多少 TCP 连接
date: 2020-12-08
author: LM
---

## 1.一台服务器最大能支持多少个网络连接？

在网络开发中，有这样一个基础问题始终没有彻底搞明白。那就是一台服务器最大究竟能支持多少个网络连接？

很多同学看到这个问题的第一反应是65535。原因是：“听说端口号最多有65535个，那长连接就最多保持65535个了”。是这样的吗？还有的人说：“应该受TCP连接里四元组的空间大小限制，算起来是200多万亿个！”

## 2.一次关于服务器端并发的聊天

![](/images/drawingbed/img/202204291725751.png)

TCP 连接四元组是指：源 IP 地址、源端口、目的 IP 地址和目的端口。

当四元组中任意一个元素发生了改变，那么就代表一条新连接出现了。

![](/images/drawingbed/img/202204291725794.png)

进程每打开一个文件（Linux 下一切皆文件，包括网络连接），都会消耗一定的内存资源。

如果有不怀好心的人启动一个进程来无限的创建和打开新的文件，那么服务器就会崩溃。所以 Linux 系统出于安全角度的考虑，在多个位置都限制了可打开的文件描述符数量，包括系统级、用户级、进程级。这三个限制的内容如下。

- 系统级：当前系统可打开的最大数量，通过`fs.file-max`参数可修改
- 用户级：指定用户可打开的最大数量，修改`/etc/security/limits.conf`
- 进程级：单个进程可打开的最大数量，通过`fs.nr_open`参数可修改

![](/images/drawingbed/img/202204291725123.png)

Linux 系统的接收缓存区大小是可以配置的，用户可以通过`sysctl`命令进行查看。

```bash
$ sysctl -a | grep rmem
net.ipv4.tcp_rmem = 4096 87380 8388608
net.core.rmem_default = 212992
net.core.rmem_max = 8388608
```

在`net.ipv4.tcp_rmem`数据中，第一个值是 TCP 连接所需分配的最少字节数。该值默认是 4K，最大支持 8MB。也就是说，Linux 在接收数据时，系统需要至少为对应的连接分配 4K 内存，甚至更大。

![](/images/drawingbed/img/202204291726583.png)

除了接收数据需要缓存区，发送数据也需要缓存区。TCP 分配发送缓存区的大小受参数`net.ipv4.tcp_wmem`配置影响。

```bash
$ sysctl -a | grep wmem
net.ipv4.tcp_wmem = 4096 65536 8388608
net.core.wmem_default = 212992
net.core.wmem_max = 8388608
```

在`net.ipv4.tcp_wmem`数据中，第一个值是发送缓存区的最小值，默认也是4K，同样支持更大。

![](/images/drawingbed/img/202204291727119.png)

## 3.服务器百万连接达成记

![](/images/drawingbed/img/202204291727222.png)

由于 Linux 对最大文件对象数量有限制，所以要想实现百万连接，用户需要在用户级、系统级、进程级等位置把文件上限加大。

![](/images/drawingbed/img/202204291727965.png)

![](/images/drawingbed/img/202204291727038.png)

用户可以通过 `ss` 命令查看当前的活动连接数量。

```bash
$ ss -n | grep ESTAB | wc -l  
1000024
```

在百万连接时，通过查看`/proc/meminfo`文件，可以看到当前机器的可用内存和缓存剩余不多。

```bash
$ cat /proc/meminfo
MemTotal:        3922956 kB
MemFree:           96652 kB
MemAvailable:       6448 kB
Buffers:           44396 kB
......
Slab:          3241244KB kB
```

通过`top`命令可以查看到`densty、flip、sock_inode_cache、TCP`四个内核对象 CPU 占用都来到了 100%。

![](/images/drawingbed/img/202204291728743.png)

![](/images/drawingbed/img/202204291728892.png)

{{< details "参考文件" >}} 
1：[ 一台Linux服务器最多能支撑多少个TCP连接？ @PHP饭米粒 ](https://mp.weixin.qq.com/s/J0Abwz20IO9N0NxooSEKXQ)
{{< /details >}}