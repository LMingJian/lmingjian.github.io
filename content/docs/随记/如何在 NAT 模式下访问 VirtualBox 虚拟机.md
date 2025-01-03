---
title: 如何在 NAT 模式下访问 VirtualBox 虚拟机
date: 2024-12-25T10:32:22+08:00
author: LiangMingJian
---

# 需求

在 NAT 模式下，VirtualBox 默认只能从虚拟机内部访问外部宿主机以及外网，而不能从外部访问内部，所以只能单向 Ping 通。现在需要在外部宿主机中访问虚拟机，完成如 SSH 登录，网页访问等工作。

# 实现

在 NAT 模式中，可以通过配置端口转发，来实现从外部访问。

在虚拟机设置中，找到网络 > 高级 > 端口转发，填写需要映射的端口（主机是外部端口，子系统是虚拟机内部端口）。

比如，虚拟机开启了 ssh 服务，服务默认服务端口为 22，则设定子系统端口为 22，主机端口 222（随意填写），那么在宿主机连接时，主机填写 localhost 或127.0.0.1，端口填写 222，即可实现宿主机 ssh 方式访问虚拟机。

比如，虚拟机开启了 httpd 服务，服务默认服务端口为 80，则设定子系统端口为 80，主机端口 880（随意填写），那么宿主机开启浏览器，输入地址 localhost:880 或 127.0.0.1:880，即实现宿主机访问虚拟机中的 httpd 提供的 web 服务了。
