---
title: 如何从零开始，启动一个别人的 Flutter 项目
date: 2025-12-27T11:47:36+08:00
author: LiangMingJian
---

# 需求

从 Github 上克隆了一个 Flutter 项目，从零开始，通过 VS Code 启动项目。

测试的项目地址：[ nisrulz / flutter-example ](https://github.com/nisrulz/flutter-examples)

# Flutter 安装

请查看另一篇文章：[ 如何从零开始，启动一个 Flutter 项目 ](https://zhuanlan.zhihu.com/p/1989017634285577333)，这里不赘述。

# 启动一个别人的 Flutter 项目

回到测试项目 [ nisrulz / flutter-example ](https://github.com/nisrulz/flutter-examples)，我们将项目克隆或者下载下来。

然后应当注意到，在这个项目中，每一个文件夹都是一个 Flutter 项目，因此，我们可以将其中任意一个想要的文件夹在 VS Code 打开。

![](_images/drawingbed/img/Pasted%20image%2020251227104139.png)

比如我们这里选中示例 `custom_home_drawer`，通过 VS Code 打开这个文件夹。

![](_images/drawingbed/img/Pasted%20image%2020251227114908.png)

## 第一步

在打开后，我们先找到 `pubspec.yaml` 查看项目的配置状态。

`pubspec.yaml` 是 Flutter 项目的核心配置文件，类似于 Android 的 Gradle 配置文件，用于管理项目的基本信息、依赖和资源等内容。

一个标准的 `pubspec.yaml` 文件应当具有以下内容：

```yaml
name: custom_home_drawer  # 项目的名称

description: A Project to demonstrate use of complex animations and gestures  # 项目的描述

publish_to: 'none'  # 发布配置，发布到 pub.dev

version: 1.0.0+1  # 版本号

environment:  # 环境约束
  sdk: ">=2.7.0 <3.0.0"  # Dart SDK 版本要求

dependencies:  # 依赖项
  flutter:  # Flutter SDK 依赖
    sdk: flutter
  cupertino_icons: ^1.0.0  # 第三方库依赖

dev_dependencies:  # 开发依赖
  flutter_test:  # 测试依赖
    sdk: flutter
```

显然，在这里我们应当首要关注 `environment` 环境约束，这里限定了这个项目支持使用的 Flutter 版本。

比如我们选择的这个测试项目，其要求 Dart 版本大于等于 2.7.0，小于 3.0.0。

![](_images/drawingbed/img/Pasted%20image%2020251230101652.png)

在知道环境限制后，我们访问如下网站：[ Flutter SDK 归档列表 ](https://docs.flutter.cn/install/archive)，找到 Stable channel，检查满足上述 Dart 版本区间的 Flutter 版本是多少，然后点击下载相应的 Flutter 版本，按文章 [ 如何从零开始，启动一个 Flutter 项目 ](https://zhuanlan.zhihu.com/p/1989017634285577333) 的介绍进行安装。

![](_images/drawingbed/img/Pasted%20image%2020251230103933.png)

因为 Dart 要小于 3.0.0，所以我们下载 Dart 2.19.6，对应 Flutter 版本 3.7.12。

在安装 Flutter 版本 3.7.12 后，我们可以执行 `flutter doctor` 检查安装状态。在一切准备好后，我们便可以回到 VS Code，执行下一步。

## 第二步

因为 Android 项目需要使用 Gradle 进行构建，而 Gradle 构建时需要从外网下载资源，因此，我们需要对 Android 项目进行镜像配置。

镜像配置的过程可以参考下述文章：[ 如何进行 Android 项目或 Flutter 项目的 Gradle 构建网络优化 ](https://zhuanlan.zhihu.com/p/1989273946588206858)

在这个项目中，首先需要修改 `android\gradle\wrapper\gradle-wrapper.properties` 的 `distributionUrl` 配置。

```
distributionUrl=https\://mirrors.aliyun.com/macports/distfiles/gradle/gradle-8.4-bin.zip
```

![](_images/drawingbed/img/Pasted%20image%2020251230114318.png)

接着，定位 `android\build.gradle` 发现里面存在配置字段 `buildscript` 和 `allprojects`，因此需要将镜像 maven 进行添加，然后又因为不是 `.kts` 文件，所以镜像 maven 要使用如下配置：

```
repositories {
    maven { url 'https://maven.aliyun.com/repository/public'}
    maven { url 'https://maven.aliyun.com/repository/central' }
    maven { url 'https://maven.aliyun.com/repository/google'}
    maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
}
```

![](_images/drawingbed/img/Pasted%20image%2020251230114620.png)

最后我们检查 `android\settings.gradle`，发现这里面没有 `pluginManagement` 或 `dependencyResolutionManagement` 这两个配置项，因此这个文件不用修改。

在完成镜像配置后，我们就可以执行下一步了。

## 第三步

在 VS Code 中，按下 `Ctrl+Shift+P` 打开命令行，输入 `flutter:get`，搜索并执行命令 `Flutter: Get Packages`，完成依赖的安装。

> 下载受网络环境影响，可能需要等待较久的时间。在完成后，会新建一个 `pubspec.lock` 文件，用来锁定当前的依赖。

![](_images/drawingbed/img/Pasted%20image%2020251230105228.png)

![](_images/drawingbed/img/Pasted%20image%2020251230114842.png)

在所有依赖成功安装后，我们便可以进行下一步，启动项目了。

## 第四步

首先，我们定位到 `lib\main.dart` 文件，找到文件中的 `main()` 方法。

![](_images/drawingbed/img/Pasted%20image%2020251230115106.png)

接着，我们需要连接到手机或者模拟器。对于 VS Code，我们点击右下角的 Device 或者 `Ctrl+Shift+P` 打开命令行，输入 `flutter:select device` 选择设备连接。

![](_images/drawingbed/img/Pasted%20image%2020251230115528.png)

![](_images/drawingbed/img/Pasted%20image%2020251230115510.png)

![](_images/drawingbed/img/Pasted%20image%2020251230133902.png)

一般来说，设备支持自动连接，但如果没有出现可选的手机或模拟器设备则可以通过 adb 命令手动连接。

```
# 检查所有已连接的设备
adb devices

# 连接到本地的模拟器设备
adb connect 127.0.0.1:7555
```

在完成连接后，便可以点击 `main()` 方法上的 Run，或者按下 F5，运行程序。

![](_images/drawingbed/img/Pasted%20image%2020251230134227.png)

![](_images/drawingbed/img/Pasted%20image%2020251230135428.png)

## 第五步（可能出现的问题）

在运行这个项目的时候，出现了下述报错。

![](_images/drawingbed/img/Pasted%20image%2020251230135525.png)

报错这里提示问题出现在 `flutter.gradle`，问题 interface（接口） `org.gradle.api.tasks.incremental.IncrementalTaskInputs` 不是一个可用参数在当前方法中。

```
* Where:
Script 'F:\Sdk\flutter3.7.12\packages\flutter_tools\gradle\flutter.gradle' line: 950

* What went wrong:
A problem occurred evaluating root project 'android'.
> A problem occurred configuring project ':app'. 
    > Could not create task ':app:copyFlutterAssetsDebug'. 
        > Could not create task ':app:mergeDebugAssets'. 
            > Cannot use @TaskAction annotation on method IncrementalTask.taskAction$gradle_core() because interface org.gradle.api.tasks.incremental.IncrementalTaskInputs is not a valid parameter to an action method.
```

显然，我们应当能想到问题可能是出现在 gradle 版本上（把报错信息扔个 AI 问问也可以）。

因此，我们点开出现问题的 `flutter.gradle` 文件，可以看见里面 `buildscript` 的 `gradle` 版本是 `7.3.0`。

![](_images/drawingbed/img/Pasted%20image%2020251230141334.png)

接着，我们便可以尝试将 `android\gradle\wrapper\gradle-wrapper.properties` 里面的 `gradle` 下载版本修改为 `7.3`，然后重新运行。

```
distributionUrl=https\://mirrors.aliyun.com/macports/distfiles/gradle/gradle-7.3-bin.zip
```

最终，程序正常运行。
