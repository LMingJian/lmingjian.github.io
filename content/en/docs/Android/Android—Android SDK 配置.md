---
title: Android SDK 配置
date: 2020-09-21
author: LMingJian
---

## 1.环境

Windows，不使用 Android Studio

## 2.配置 Android SDK

1. 从 [Android Studio 下载页面](https://developer.android.google.cn/studio?hl=zh-cn) 中下载最新的`command line tools only`软件包，然后将其解压缩。
2. 将解压缩的 `cmdline-tools` 文件夹移至您另外创建一个文件夹内，例如 android_sdk。这个新目录就是您的 Android SDK 目录。
3. 在解压缩的 `cmdline-tools` 目录中，创建一个名为 `latest` 的子目录。
4. 将原始 `cmdline-tools` 目录内容（包括 `lib` 目录、`bin` 目录、`NOTICE.txt` 文件和 `source.properties` 文件）移动到新创建的 `latest` 目录中。现在，您就可以从这个位置使用命令行工具了。（可以将 `android_sdk\cmdline-tools\latest\bin\sdkmanager.bat` 添加到环境变量中 ）
5. 在环境变量中配置 SDK 路径，如 `ANDROID_HOME=D:/android_sdk`
6. 执行命令 `sdkmanager "platform-tools" "platforms;android-28" "build-tools;28.0.0"`
7. 等待下载完成后，Android SDK 配置成功，在终端中执行 `android`，能成功输出内容。

## 3.sdkmanager

`sdkmanager` 是一个命令行工具，用户可以用它来查看、安装、更新和卸载 Android SDK 的软件包，而不用下载 Android Studio。

```cmd
# 卸载软件包
sdkmanager --uninstall packages [options]
sdkmanager --uninstall --package_file=package_file [options]
# 列出已安装和可用的软件包
sdkmanager --list [options] \
           [--channel=channel_id] // Channels: 0 (stable), 1 (beta), 2 (dev), or 3 (canary)
# 更多请查看参考文档
```

> 参考文档：[ sdkmanager @Android Studio 用户指南 ](https://developer.android.google.cn/studio/command-line/sdkmanager)

