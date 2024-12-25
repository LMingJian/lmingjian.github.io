---
title: 如何给 Flutter 的 APK 瘦身
date: 2024-12-25T11:20:35+08:00
author: LiangMingJian
---

# 根据不同的环境打包

Flutter 在打包编译时，会根据 CPU 架构（x86_64、arm64-v8a、armeabi-v7a）的不同打出对应的二进制库，体现在 apk 的 lib 文件中。因此，要想将打包出的 apk 进行瘦身，可以将程序按不同架构进行打包。

```bash
flutter build apk --release --target-platform android-arm  # android-arm 架构
flutter build apk --target-platform android-arm,android-arm64,android-x64 --split-per-abi # 分别对 android-arm,android-arm64,android-x64 架构进行打包
```

PS：x86_64 一般是模拟器和平板使用， arm64-v8a 就是 arm 64 位，大部分新手机使用。 armeabi 虽然不是现在主流的 CPU 架构，但是高版本的都会对其进行兼容，所以主流的 App 如果只想发一个版本的上线，可以只使用 armeabi 。
