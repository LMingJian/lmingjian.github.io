---
title: 修复 Android 访问双目摄像头时 ID 随机变动的问题
date: 2024-12-25T11:00:46+08:00
author: LiangMingJian
---

# BUG 描述

Android 应用在调取双目摄像头画面进行预览时，有时会出现画面异常的问题。比如双目摄像头分为普通摄像头和红外摄像头，用户需要调用普通摄像头，正常情况下调用成功，但某些情况下会调用成红外摄像头。

# Resolution

由于某些厂商原因在启动设备时，未对摄像头加载顺序进行控制，这就导致设备重启后摄像头的加载顺序可能会有变动。

比如正常情况下，系统检测到设备摄像头数量有 2 个，分别标注为 bgr Camera id 为 0，ir Camera id 为 1，用户调用 0 既可以调用正常的 bgr Camera。但某些情况下，由于加载顺序变动，此时 bgr Camera id 变为 1，ir Camera id 为 0，此时，若用户调用 0 则会调用成异常的 ir Camera。

为解决该问题，用户可以使用 shell 命令查询 usb 设备列表信息，查询设备加载顺序，然后根据摄像头加载顺序对预览的摄像头 ID 进行调换。

{{< details "参考文件" >}} 
1：[ Android 处理双目摄像头 ID 随机变动问题  @Mocaris的博客 ](https://blog.csdn.net/ZP313689969/article/details/130362218)
{{< /details >}}
