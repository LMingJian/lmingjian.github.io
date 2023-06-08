---
title: APK Analyzer 工具
date: 2021-06-09
author: LMingJian
---

## 1.APK Analyzer 

APK Analyzer 是 Android Studio 提供的 APK 包分析工具，可以打开并审查存于你电脑中的 APK 文件的内容。APK Analyzer 是一个非常好的工具来分析 APK 文件的结构，并同时在发布前或调试时验证一些常见问题，例如 APK 大小和 DEX 问题。

APK Analyzer 可以在 Android Studio 顶端菜单栏中的 Build 找到。

## 2.利用 APK Analyzer 为应用瘦身

APK Analyzer 在应用大小方面可以给你很多有用并且可操作的信息。比如你可以从 Raw File Size 看到应用占磁盘大小。Download size 是一个估计值，表示考虑到在经过 Play Store 的压缩后，你还需要多少流量来下载应用。

文件和文件夹根据文件大小降序排列。这让我们很容易看出对 APK 大小优化最容易从哪里入手。每当你深入到某个文件夹的时候，你能看到占用了 APK 大部分空间的资源和其他实体。资源根据文件大小以降序的方式排列。

> 参考文档：[ 利用好 Android Studio 中的 APK Analyzer  @Glowin ](https://zhuanlan.zhihu.com/p/24262346)

