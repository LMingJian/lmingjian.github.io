---
title: Dart 的睡眠
date: 2021-04-27
author: LMingJian
---

## 1.睡眠

在异步代码中睡眠。

```dart
await Future.delayed(Duration(seconds: 1));
```

在同步代码中睡眠。

```dart
import 'dart:io';
sleep(Duration(seconds:1));
```

