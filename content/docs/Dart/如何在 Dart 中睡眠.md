---
title: 如何在 Dart 中睡眠
date: 2024-12-25T11:05:58+08:00
author: LiangMingJian
---

# 睡眠

在异步代码中睡眠。

```dart
await Future.delayed(Duration(seconds: 1));
```

在同步代码中睡眠。

```dart
import 'dart:io';
sleep(Duration(seconds:1));
```
