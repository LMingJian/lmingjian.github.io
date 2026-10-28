---
title: 记一次 Docker 网络服务冲突问题
date: 2026-10-10T14:53:08+08:00
author: LiangMingJian
---

# 现场

现在有 AB 两个区域，A 区域是设备，B 区域是服务器，中间相隔很多层路由。正常情况下，A 区域设备和 B 区域服务器互通，相互可以正常访问。

但是，在安装 Docker 服务，并启用相关容器后，A 区域和 B 区域忽然断开了，相互之间不能互相访问。

# 探索

A 区域设备 IP 是 172.17.5.65，B 区域服务器 IP 是 172.18.1.91。

对于这种无法访问的问题，第一个步骤我建议是使用**路由跟踪指令**从两端看看网络卡在哪里了。

在 Windows 上，我们可以使用 `tracert` 指令。使用 `tracert 172.18.1.91`。

在 Linux 上，我们可以使用 `traceroute` 指令。使用 `traceroute 172.17.5.65`。

> 在最小安装的 Linux 上，可能没有 `traceroute`，这时可以使用 `tracepath` 指令，达到类似效果。

在测试中，服务器跟踪路由到设备时，发现了以下问题：

![](_images/drawingbed/img/Pasted%20image%2020261010150831.png)

我们关注到 `localhost.localdomain`，显然，路由跳转在服务器本机就直接卡住了，有一个 IP 是 `172.17.0.1` 的服务把数据全接收并丢弃了。

设备和服务器原本正常互通的，只是在安装 Docker 后异常了，我们应该有理由怀疑是 Docker 服务造成的影响。

我们查看 Docker 服务的网络 `docker network ls`，然后逐个检测各个网桥的 IP 域 `docker network inspect <网络名称>`。

![](_images/drawingbed/img/Pasted%20image%2020261010151410.png)

问题显而易见，Docker 有一个网桥的 IP 域，`172.17.0.0/16` 这个域和现场局域网冲突了，所以导致问题的出现。

> Docker 默认使用的域是 `172.17.0.0/16`，如果没有在创建时指定，Docker 会默认使用这个域。

问题定位了，解决也就容易了。

# 解决

既然是冲突，那么只要把 Docker 网桥的 IP 域改成别的就可以了。

详细操作步骤见文章：[通过固定 Host 解决 Docker 网络内部 DNS 异常的问题](https://zhuanlan.zhihu.com/p/2029504233259509146)
