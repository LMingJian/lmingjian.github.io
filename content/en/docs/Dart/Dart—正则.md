---
title: 正则处理
date: 2021-04-25
author: LMingJian
---

## 1.Dart 的正则处理

在 Dart 中，通过构建类`RegExp(r"正则表达式")`来对数据进行正则处理。

## 2.示例

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