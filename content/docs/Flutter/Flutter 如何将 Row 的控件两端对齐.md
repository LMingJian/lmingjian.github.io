---
title: Flutter 如何将 Row 的控件两端对齐
date: 2024-12-25T13:46:06+08:00
author: LiangMingJian
---

# 使用 spaceBetween 对齐方式

在 Row 组件中使用 `MainAxisAlignment.spaceBetween`实现两端对齐。

```dart
new Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: [
    new Text("left"),
    new Text("right")
  ]
);
```

# 中间使用 Expanded 自动扩展

在 Row 组件的子控件中使用 `Expanded(child: SizedBox())` 填充空白，实现两端对齐。

```dart
Row(
  children: <Widget>[
    FlutterLogo(),//左对齐
    Expanded(child: SizedBox()),//自动扩展挤压
    FlutterLogo(),//右对齐
  ],
);
```

# 使用 Spacer 自动填充

在 Row 组件的子控件中使用 `Spacer()` 填充空白，实现两端对齐。

```dart
Row(
  children: <Widget>[
    FlutterLogo(),
    Spacer(),
    FlutterLogo(),
  ],
);
```

# 使用 Flexible

在 Row 组件的子控件中使用 `Flexible(fit: FlexFit.tight, child: SizedBox())` 填充空白，实现两端对齐。

```dart
Row(
  children: <Widget>[
    FlutterLogo(),
    Flexible(fit: FlexFit.tight, child: SizedBox()),
    FlutterLogo(),
  ],
);
```
