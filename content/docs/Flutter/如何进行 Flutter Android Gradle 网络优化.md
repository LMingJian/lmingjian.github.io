---
title: 如何进行 Flutter Android Gradle 网络优化
date: 2024-12-25T11:21:03+08:00
author: LiangMingJian
---

# 需求

运行 Flutter 或 Android 项目后，终端一直卡在：`Running Gradle task 'assembleDebug'...`，或在这个后提示连接被拒绝。

# 原因

Flutter Android 项目，或者说 Android 项目会在编译项目前检查项目配置，然后从远端服务器下载所需资源。因此，受远端服务器影响，所需时间可能会很长，同时界面不会有任何提示，所以用户会觉得程序卡住，无法执行。

此时，用户可以打开任务管理器，检查 IDE 或 JDK 的下载流量，以此判断程序是否在执行。

但最好的解决办法是替换镜像源，来加快资源下载速度。（因为资源大部分都在国外，因此需要进行镜像加速）

# 解决方案

**配置 Flutter 资源库镜像**

在系统环境变量中添加以下内容。

```
PUB_HOSTED_URL="https://pub.flutter-io.cn" 
FLUTTER_STORAGE_BASE_URL="https://storage.flutter-io.cn"
```

**配置项目 Gradle 的下载地址**

访问项目文件 `android\gradle\wrapper\gradle-wrapper.properties`，修改 `distributionUrl` 这个值。

```
# 原来的配置
distributionUrl=https\://services.gradle.org/distributions/gradle-8.14-all.zip

# 使用腾讯云加速
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.14-all.zip

# 使用阿里云加速
distributionUrl=https\://mirrors.aliyun.com/macports/distfiles/gradle/gradle-8.14-all.zip
```

**配置 `build.gradle` 或 `settings.gradle` 文件**

定位文件 `android\build.gradle` 和 `android\settings.gradle`。

在 `build.gradle` 中找到 `buildscript` 或 `allprojects` 这两个配置项。

在 `settings.gradle` 中找到 `pluginManagement` 或 `dependencyResolutionManagement` 这两个配置项。

在上面这些配置项中，找到 `repositories` 这个值，在值内加入以下的镜像地址。

```groovy
repositories {
	maven { url 'https://maven.aliyun.com/repository/public'}
    maven { url 'https://maven.aliyun.com/repository/central' }
    maven { url 'https://maven.aliyun.com/repository/google'}
    maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
}
```

**配置 `build.gradle.kts` 或 `settings.gradle.kts` 文件**（在较新版本的 Flutter Android 项目）

定位文件 `android\build.gradle.kts` 和 `android\settings.gradle.kts`。

在 `build.gradle.kts` 中找到 `buildscript` 或 `allprojects` 这两个配置项。

在 `settings.gradle.kts` 中找到 `pluginManagement` 或 `dependencyResolutionManagement` 这两个配置项。

在上面这些配置项中，找到 `repositories` 这个值，在值内加入以下的镜像地址。

> 在 `.kts` 这类 Kotlin 语言文件中，这里的 maven 被写成了一个方法，因此需要使用 `url=xxx` 键值对来传递参数。

```kotlin
repositories {
	maven(url = "https://maven.aliyun.com/repository/public/") 
	maven(url = "https://maven.aliyun.com/repository/central") 
	maven(url = "https://maven.aliyun.com/repository/google") 
	maven(url = "https://maven.aliyun.com/repository/gradle-plugin")
}
```
