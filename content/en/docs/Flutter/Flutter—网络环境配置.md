---
title: Flutter 网络环境配置
date: 2021-04-25
author: LM
---

## 1.背景

在执行 `flutter doctor` 时，最后一项检查项目为 Network Resources，检查 Flutter 运行过程中依赖的网络资源来源站是否可以正常访问，这些网站包括 maven.google.com，谷歌的 maven 库；pub.dev，Dart 和 Flutter 的官方包库；github.com，最大的项目开源库。

在中国，这些资源站往往都是访问不了的。

## 2.解决 maven.google.com 的访问

- 首先找到 Flutter SDK 的位置。
- 然后定位到 `\packages\flutter_tools\lib\src\http_host_validator.dart` 并打开文件。
- 接着修改文件内容，将 maven.google.com 修改为镜像库 dl.google.com/dl/android/maven2 。
- 最后删除 bin 目录下的 cache 缓存文件，重新运行 `flutter doctor`。

## 3.解决 pub.dev 的访问

在终端命令或 Windows 环境变量中增加键值，将环境变量指向国内的镜像库。

```bash
$env:PUB_HOSTED_URL="https://pub.flutter-io.cn"
$env:FLUTTER_STORAGE_BASE_URL="https://storage.flutter-io.cn"
```

## 4.解决 pub.dev 的访问

挂代理，或使用如 Steam++ 的这类加速器。

{{< details "参考文件" >}} 
1：[ 在中国网络环境下使用 Flutter - @Flutter 中文开发者网站 ](https://flutter.cn/community/china)
2：[ flutter doctor network resources 报错，解决国内开发环境问题 - @简书 ](https://www.jianshu.com/p/b69231defaaf)
{{< /details >}}