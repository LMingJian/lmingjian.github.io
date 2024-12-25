---
title: 修复 Flutter Running Gradle task 卡住的问题
date: 2024-12-25T11:21:03+08:00
author: LiangMingJian
---

# 报错

运行 Flutter 项目后，终端一直卡在：`Running Gradle task 'assembleDebug'...`。

# 原因

Flutter 在编译项目前会检查项目配置，然后从远端服务器下载所需资源，受远端服务器影响，所需时间可能会很长，因此用户会觉得程序卡住，无法执行。用户可以打开任务管理器，检查 IDE 或 JDK 的下载流量，以此判断程序是否在执行。

# 解决方案

1.修改 Flutter 镜像：在环境变量中修改。

```
PUB_HOSTED_URL=https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

2.修改 Gradle 镜像：在项目文件`android\build.gradle`中修改如下内容，添加镜像`maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}`。

```
buildscript {
    .....
    repositories
 		maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
		mavenCentral()
    }
    .....
}
.............
allprojects {
	repositories {
		maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
		mavenCentral()
	}
}
```
