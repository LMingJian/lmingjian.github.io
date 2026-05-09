---
title: 什么是 adb（Android 调试桥）
date: 2024-12-25T11:00:41+08:00
author: LiangMingJian
---

# 概述

Android 调试桥（adb）是一种功能多样的命令行工具，可以让你与设备进行通信。

adb 提供对 Unix shell（可用来在设备上运行各种命令，例如日志打印，应用安装，应用调试等）的访问权限。

adb 是一种【客户端-服务器】程序，它包括以下三个组件：

- 客户端：用于发送命令。客户端在开发机器上运行，通过输入 adb 命令从命令行终端调用客户端。
- 守护程序（adbd）：用于在设备上运行命令。守护程序在每个设备上作为后台进程运行。
- 服务器：用于管理客户端与守护程序之间的通信。服务器在开发机器上作为后台进程运行。

adb 默认包含在 Android SDK 平台工具软件包中。您可以使用 [ sdk manager（SDK 管理器）](https://developer.android.google.cn/studio/intro/update?hl=zh-cn#sdk-manager) 下载此软件包，该管理器会将其安装在 `android_sdk/platform-tools/` 目录下。

> 如果您只是需要独立的 Android SDK 平台工具软件包，请从这里单独下载 [ platform-tools（SDK 平台工具）](https://developer.android.google.cn/studio/releases/platform-tools?hl=zh-cn)。

# 连接 adb

## 通过 USB 连接

当设备在**系统设置** > **开发者选项**中启用**USB 调试**后，设备便可以通过 USB 进行连接。

> **注意**，在 Android 4.2（API 级别 17）及更高版本中，**开发者选项**默认处于隐藏状态。如需将其显示出来，请在**设置** > **关于手机**中点击多次的版本号，直到出现“**你已经在开放者模式了**”。此时，返回上一界面，可以在底部找到**开发者选项**。

当设备通过 USB 连接到计算机时，adb 便自动完成连接工作，用户可以通过在命令行终端执行 `adb devices` 来检查设备是否连接。

> **注意**：当你连接搭载 Android 4.2.2（API 级别 17）或更高版本的设备时，设备会显示一个对话框，询问您是否接受允许通过此计算机进行调试的 RSA 密钥，**你需要点击确认**。这种安全机制可以保护用户设备，确保用户只有在能够解锁设备并确认对话框的情况下才能执行 USB 调试和其他 adb 命令。

## 通过网络连接

当设备开启**无线调试**功能后，用户便可以通过网络远程连接 adb。

用户需要保证设备与计算机处于同一网络，且设备与计算机间的网络互通，然后，在计算机命令行终端中执行 `adb connect ip:5555` 完成 adb 连接工作（端口默认 5555）。

> 注意，部分设备 5555 端口可能默认不开放，因此需要先用 USB 连接，然后再通过执行 `adb tcpip 5555` 开放。

另外，在 Android 11（API 级别 30）及更高版本中，无线连接需要使用 `adb pair ip:port` 这个命令，连接的同时还需要提供配对码。设备连接的端口和配对码可以通过**无线调试设置**中的**使用配对码配对设备**（Pair device with pairing code）进行展示。

![](_images/drawingbed/img/Pasted%20image%2020260508143902.png)

```shell
adb pair 192.168.1.130:37099
Enter pairing code: 482924
Successfully paired to 192.168.1.130:37099 [guid=adb-235XY]
```

# 常用的 adb 指令

- 获取连接设备：`adb devices`
- 获取连接设备（详细）：`adb devices -l`
- 获取设备日志：`adb logcat`，`adb logcat *:W`

> logcat 命令支持过滤语法，相关的语法操作请查看 [ Logcat 命令行工具 ](https://developer.android.google.cn/tools/logcat?hl=zh-cn)，在此不赘述。

- 安装应用：`adb install path_to_apk`
- 从设备复制文件：`adb pull remote local`，`adb pull /sdcard/myfile.txt myfile.txt`
- 复制文件到设备：`adb push local remote`，`adb push myfile.txt /sdcard/myfile.txt`

——————————

更多的 adb 使用，请阅读下述的用户指南。

> 参考文件：[ adb @Android Studio 用户指南 ](https://developer.android.google.cn/tools/adb?hl=zh-cn)
