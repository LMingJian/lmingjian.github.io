---
title: Flutter 组件 CupertinoPicker
date: 2021-05-18
author: LM
---

## 1.组件使用

Flutter 组件 CupertinoPicker 是一个滚轮选择器，用户可以通过滚动在给出选项中进行选择。

```dart
Container(
    height: 120,
    width: 100,
    child: CupertinoPicker(
        itemExtent: 27,
        onSelectedItemChanged: (position) {},
        children: [ Text("192") ]
    ),),
```

