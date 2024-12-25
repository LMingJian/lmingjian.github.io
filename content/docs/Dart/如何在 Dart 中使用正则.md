---
title: 如何在 Dart 中使用正则
date: 2024-12-25T11:05:53+08:00
author: LiangMingJian
---

# 概述

在 Dart 中，通过构建类`RegExp(r"正则表达式")`来对数据进行正则处理。

# 示例

```dart
RegExp exp = RegExp(r"(\w+)");
String str = "Parse my string";
Iterable<Match> matches = exp.allMatches(str);
for (Match m in matches) {
  String? match = m.group(0);
  print(match);
}
// 搜索并返回第一个匹配项，没有则返回null
RegExp exp = RegExp(r"(\d)");
print(exp.firstMatch("aaaa1bb2cc3dd")!.group(0));
// 是否存在匹配项
RegExp exp = RegExp(r"(\d)");
print(exp.hasMatch("aaaaaaaa"));
```
