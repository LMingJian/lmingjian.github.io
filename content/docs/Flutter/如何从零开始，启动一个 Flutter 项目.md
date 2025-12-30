---
title: 如何从零开始，启动一个 Flutter 项目
date: 2025-12-27T09:07:24+08:00
author: LiangMingJian
---

# 需求

从零开始，通过 VS Code 启动一个 Flutter 项目。

# 安装 Flutter

（**推荐**）访问中国的镜像教程地址：[ Flutter Docs ](https://docs.flutter.cn/get-started/quick)，阅读教程并根据教程完成 Flutter 安装。

> 访问 Flutter 官方教程地址：[ Flutter Docs ](https://docs.flutter.dev/get-started/quick)，阅读教程并根据教程完成 Flutter 安装。

## 1.确认平台（Confirm your development platform）

**本文以 Windows 为例**。

Windows 平台下，Flutter 要求先安装 Git 与 VS Code 环境。

![](_images/drawingbed/img/Pasted%20image%2020251227092131.png)

## 2.安装 Flutter（Install and set up Flutter）

Flutter 的安装分为两步。

**第一步，在 VS Code 中安装相应的拓展**。

![](_images/drawingbed/img/Pasted%20image%2020251227093902.png)

![](_images/drawingbed/img/Pasted%20image%2020251227094105.png)

![](_images/drawingbed/img/Pasted%20image%2020251227094123.png)

**第二步，安装 Flutter SDK**。

在 VS Code 中，使用 `Control + Shift + P` 打开命令输入器，输入关键字 `flutter`，在可用的选项中选择 `Flutter: New Project`。

![](_images/drawingbed/img/Pasted%20image%2020251227094500.png)

此时，VS Code 会自行检测系统是否具备 Flutter SDK 环境，如果系统不存在该环境，则弹出一个提示，要求用户下载或选择本地 SDK 加载。

我们选择 `Download SDK`，下载 Flutter SDK 环境。

![](_images/drawingbed/img/Pasted%20image%2020251227094644.png)

在点击 `Download SDK` 后，VS Code 会打开一个文件选择框，要求用户选择 SDK 的存放位置。在选择完成后，点击 `Clone Flutter` 即可。

![](_images/drawingbed/img/Pasted%20image%2020251227094900.png)

接着，VS Code 左下角就会出现 Flutter SDK 的下载提示，请等待 SDK 下载完成。

![](_images/drawingbed/img/Pasted%20image%2020251227095050.png)

> 请注意，在国内环境，下载 Flutter SDK 有可能因为网络而导致失败，因此一般建议多尝试几次，或者使用手动下载的方式。

> 手动下载时，可以访问如下地址：[ Flutter Archive ](https://docs.flutter.cn/install/archive)，下载 Stable channel 相应版本的 SDK，解压然后放到一个位置。

![](_images/drawingbed/img/Pasted%20image%2020251227100044.png)

在下载完成后，关闭 VS Code，定位 SDK 的位置，然后在系统环境变量 PATH 中添加。

> 系统环境变量 PATH 可以通过 `Win+ S` 打开搜索，然后找到`编辑系统环境变量`，在 `Path` 中，添加 SDK 位置，如 `F:\SDK\flutter\bin`

![](_images/drawingbed/img/Pasted%20image%2020251227100346.png)

![](_images/drawingbed/img/Pasted%20image%2020251227101843.png)

在添加完成系统变量后，打开 Windows 终端或 CMD，输入 `flutter -h` 查看 flutter 是否安装完成。

![](_images/drawingbed/img/Pasted%20image%2020251227102326.png)

# 在国内环境使用 Flutter（配置镜像加速）

在中国，由于网络问题，在使用 Flutter 时，往往需要配置镜像加速，以加快相关软件包的下载。

参照文档 [ 在中国网络环境下使用 Flutter ](https://docs.flutter.cn/community/china/) 的介绍，应当在系统的环境变量中，添加以下两个值：

```dart
PUB_HOSTED_URL="https://pub.flutter-io.cn"
FLUTTER_STORAGE_BASE_URL="https://storage.flutter-io.cn"
```

![](_images/drawingbed/img/Pasted%20image%2020251227103105.png)

# 启动一个全新的 Flutter 项目

打开 VS Code，使用 `Control + Shift + P` 打开命令输入器，输入关键字 `flutter`，在可用的选项中选择 `Flutter: New Project`，创建一个标准的 Flutter 项目。

项目模板选择 `Application`，项目地址随意，项目名称 example（必须全部小写），最后按下 `Enter 回车`，创建项目。

![](_images/drawingbed/img/Pasted%20image%2020251227105827.png)

![](_images/drawingbed/img/Pasted%20image%2020251227105841.png)

![](_images/drawingbed/img/Pasted%20image%2020251227105906.png)

![](_images/drawingbed/img/Pasted%20image%2020251227105925.png)

![](_images/drawingbed/img/Pasted%20image%2020251227110049.png)

Flutter 支持运行在很多的平台上，我们可以通过在 `Control + Shift + P` 中输入 `Flutter: Select Device` 选择平台，也可以在 VS Code 右下角选择平台。

![](_images/drawingbed/img/Pasted%20image%2020251227111520.png)

最后，打开 `lib\main.dart`，这里就是**整个项目的主要代码文件**，找到主函数 `void main()`，点击运行 `run`，启动项目。

![](_images/drawingbed/img/Pasted%20image%2020251227110740.png)

当然，点击 VS Code 侧边栏运行，或按下 `F5` 运行，也是一种方式。

![](_images/drawingbed/img/Pasted%20image%2020251227111738.png)

成功运行后，程序会在对应的平台上展示，比如这里选择的浏览器。

![](_images/drawingbed/img/Pasted%20image%2020251227110657.png)

# 启动一个全新的安卓项目

Flutter 项目如果需要运行在安卓手机上，那么系统就必须具有 Android SDK。

**Android SDK 的安装请参照文章**：[ 如何不通过 Android Studio 安装 Android SDK ]()

在确认安装 Android SDK 后，我们再次打开 VS Code，使用 Ctrl+Shift+\` 打开控制台终端。

我们首先执行 `flutter doctor`，检查 Android SDK 是否满足要求。

![](_images/drawingbed/img/Pasted%20image%2020251229153505.png)

由图所示，这里出现了两个问题，第一个是：

```python
Flutter requires Android SDK 36 and the Android BuildTools 28.0.3
```

Android SDK 最低要 36 和 Android BuildTools 最低要 28.0.3，因此我们需要安装这两个版本的功能，使用 sdkmanager 执行：

```python
sdkmanager "platforms;android-36" "build-tools;36.0.0"
```

第二个问题是：

```python
Some Android licenses not accepted. To resolve this, run: flutter doctor --android-licenses
```

显然，它这里有提示执行 `flutter doctor --android-licenses`，因此，在终端执行确认即可。

![](_images/drawingbed/img/Pasted%20image%2020251229153904.png)

再次执行 `flutter doctor`，看到 Android toolchain 正常了。

![](_images/drawingbed/img/Pasted%20image%2020251229154008.png)

参照上一个章节【启动一个全新的 Flutter 项目】，创建项目后，我们在右下角找到我们的手机或者模拟器，点击连接。

![](_images/drawingbed/img/Pasted%20image%2020251229154201.png)

**请注意，在国内环境，下面的配置是重点**。

在国内，安卓项目的部署会要求下载 Gradle，同时会使用 Maven 下载相应的依赖文件，但因为网络问题，往往无法进行，此时，我们就需要进行镜像地址配置。

在 Flutter 安卓项目中，应当在以下三个地方配置镜像文件。

**第一**：`android\gradle\wrapper\gradle-wrapper.properties`，在这个文件中，我们需要将 `distributionUrl` 这个的属性值由 `https\://services.gradle.org/distributions` 修改成 `https\://mirrors.aliyun.com/macports/distfiles/gradle`，由谷歌下载换成阿里云下载。

![](_images/drawingbed/img/Pasted%20image%2020251229162348.png)

**第二**：如果有文件 `android\settings.gradle.kts`，则要在这个文件中寻找是否有 `pluginManagement` 或 `dependencyResolutionManagement` 这两个配置项，需要在这两个配置项中的 `repositories` 添加以下配置值。（如果没有文件则不用管，如果只有 `pluginManagement`，则只在 `pluginManagement` 中添加）

> **注意，如果文件后缀不是 `.kts` 这类 Kotlin 语言文件**，如 `settings.gradle`，那么这里的  maven 方法应当写成 `maven { url 'https://maven.aliyun.com/repository/central' }`，不能写成 `maven(url = "https://maven.aliyun.com/repository/central")`

最终的配置项应当类似于：

```json
pluginManagement {
	...
	repositories { 
		maven(url = "https://maven.aliyun.com/repository/public/") 
		maven(url = "https://maven.aliyun.com/repository/central") 
		maven(url = "https://maven.aliyun.com/repository/google") 
		maven(url = "https://maven.aliyun.com/repository/gradle-plugin")
		
		google() 
		mavenCentral() 
		gradlePluginPortal() 
	} 
} 

dependencyResolutionManagement { 
	...
	repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS) 
	repositories { 
		maven(url = "https://maven.aliyun.com/repository/public/") 
		maven(url = "https://maven.aliyun.com/repository/central") 
		maven(url = "https://maven.aliyun.com/repository/google") 
		maven(url = "https://maven.aliyun.com/repository/gradle-plugin") 
		
		google() 
		mavenCentral() 
	} 
} 

...
include(":app")
```

![](_images/drawingbed/img/Pasted%20image%2020251229162924.png)

**第三**：如果有文件 `android\build.gradle.kts`，则要在这个文件中寻找是否有 `buildscript` 或 `allprojects` 这两个配置项，需要在这两个配置项中的 `repositories` 添加以下配置值。（如果没有文件则不用管，如果只有 `allprojects`，则只在 `allprojects` 中添加）

> **注意，如果文件后缀不是 `.kts` 这类 Kotlin 语言文件**，如 `build.gradle`，那么这里的 maven 方法应当写成 `maven { url 'https://maven.aliyun.com/repository/central' }`，不能写成 `maven(url = "https://maven.aliyun.com/repository/central")`

最终的配置项应当类似于：

```json
buildscript {
	...
    repositories {        
	    maven(url = "https://maven.aliyun.com/repository/public/") 
		maven(url = "https://maven.aliyun.com/repository/central") 
		maven(url = "https://maven.aliyun.com/repository/google") 
		maven(url = "https://maven.aliyun.com/repository/gradle-plugin")
	    
        google()
    }
    ...
}

allprojects {
	...
    repositories {
	    maven(url = "https://maven.aliyun.com/repository/public/") 
		maven(url = "https://maven.aliyun.com/repository/central") 
		maven(url = "https://maven.aliyun.com/repository/google") 
		maven(url = "https://maven.aliyun.com/repository/gradle-plugin")
	    
        google()
    }
    ...
}
```

最后，点击 `lib/main.dart` 中主函数 `main()` 上的 Run，或者按下 F5，启动代码运行。

![](_images/drawingbed/img/Pasted%20image%2020251229164406.png)
