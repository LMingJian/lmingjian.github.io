---
title: APK Analyzer 工具
date: 2021-06-09
author: LMingJian
---

## 1.APK Analyzer 

APK Analyzer 是 Android Studio 提供的 APK 包分析工具，可以打开并审查存于你电脑中的 APK 文件的内容。APK Analyzer 可以用来分析 APK 文件的结构，并同时在发布前或调试时验证一些常见问题，例如 APK 大小和 DEX 问题。

APK Analyzer 可以在 Android Studio 顶端菜单栏中的 Build 找到。

## 2.利用 APK Analyzer 为应用瘦身

APK Analyzer 在分析 APP 大小时，提供了很多有用且可操作的信息。比如，你可以从 Raw File Size 看到 APP 占磁盘大小，或者查看在经过 Play Store 的压缩后，你还需要多少流量（Download size）来下载。

文件和文件夹也会根据文件大小降序排列，这让我们很容易看出 APK 数据的占用情况。此外，每当你深入到某个文件夹的时候，这个文件夹内的所有资源和其他实体都会根据文件大小以降序的方式排列，以便我们对 APK 大小进行优化。

{{< details "参考文件" >}} 
1：[ 利用好 Android Studio 中的 APK Analyzer  @Glowin ](https://zhuanlan.zhihu.com/p/24262346)
{{< /details >}}

