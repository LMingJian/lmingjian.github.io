---
title: Linux Command：reboot
date: 2024-12-25T13:58:19+08:00
author: LiangMingJian
---

# 概述

reboot 命令用于用来重新启动计算机。

# 支持的参数

```
-n : 在重开机前不做将记忆体资料写回硬盘的动作
-w : 并不会真的重开机，只是把记录写到 /var/log/wtmp 档案里
-d : 不把记录写到 /var/log/wtmp 档案里（-n 这个参数包含了 -d）
-f : 强迫重开机，不呼叫 shutdown 这个指令
-i : 在重开机之前先把所有网络相关的装置先停止
```
