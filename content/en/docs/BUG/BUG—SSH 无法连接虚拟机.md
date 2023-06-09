---
title: SSH 无法连接虚拟机
date: 2020-12-02
author: LMingJian
---

## BUG 描述

无法使用 SSH 连接虚拟机，使用 Ping 指令，能从虚拟机中 Ping 通主机，但不能从外部主机 Ping 通虚拟机。

## Resolution

注意网关配置的问题，查看网关是否错误，可以打开网络适配器，右键虚拟网卡，诊断，重置网卡，重新配置网关。

