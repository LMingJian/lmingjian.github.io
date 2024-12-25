---
title: 如何安装 Android SDK
date: 2024-12-25T11:00:34+08:00
author: LiangMingJian
---

# 使用 sdkmanager  配置 SDK

1. 从 [Android Studio 下载页面](https://developer.android.google.cn/studio?hl=zh-cn) 中下载最新的`command line tools only`软件包，然后将其解压缩。
2. 将解压缩的 `cmdline-tools` 文件夹移至您另外创建一个文件夹内，例如 `android_sdk`。这个新目录就是您的 SDK 目录。
3. 在解压缩的 `cmdline-tools` 目录中，创建一个名为 `latest` 的子目录。
4. 将原始 `cmdline-tools` 目录内容（包括 `lib` 目录、`bin` 目录、`NOTICE.txt` 文件和 `source.properties` 文件）移动到新创建的 `latest` 目录中。
5. 现在，您就可以从这个位置使用命令行工具。（也可以将 `android_sdk\cmdline-tools\latest\bin\sdkmanager.bat` 添加到环境变量中 ）
6. 在环境变量中配置 SDK 路径，如 `ANDROID_HOME=D:/android_sdk`
7. 执行命令 `sdkmanager "platform-tools" "platforms;android-28" "build-tools;28.0.0"`
8. 等待下载完成后，Android SDK 配置成功，在终端中执行 `android`，能成功输出内容。

# 什么是 sdkmanager

`sdkmanager` 是一个命令行工具，用户可以用它来查看、安装、更新和卸载 Android SDK 的软件包，而不用下载 Android Studio。

```bash
# 卸载软件包
sdkmanager --uninstall packages [options]
sdkmanager --uninstall --package_file=package_file [options]
# 列出已安装和可用的软件包
sdkmanager --list [options] \
           [--channel=channel_id] // Channels: 0 (stable), 1 (beta), 2 (dev), or 3 (canary)
```

{{< details "参考文件" >}} 
1：[ sdkmanager @Android Studio 用户指南 ](https://developer.android.google.cn/studio/command-line/sdkmanager?hl=zh-cn)
{{< /details >}}
