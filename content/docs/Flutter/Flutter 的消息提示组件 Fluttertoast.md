---
title: Flutter 的消息提示组件 Fluttertoast
date: 2024-12-25T11:21:46+08:00
author: LiangMingJian
---

# Fluttertoast

Flutter 组件 Fluttertoast 是一个第三方的消息提示组件，在 pubspec.yaml 文件中添加依赖。

```yaml
fluttertoast: ^4.0.1
```

# 使用

```dart
import 'package:fluttertoast/fluttertoast.dart';

RaisedButton(
    child: Text("弹出toast"),
    onPressed: () {
        Fluttertoast.showToast(
            msg: "你今天真好看",
            toastLength: Toast.LENGTH_SHORT,
            gravity: ToastGravity.BOTTOM,
            timeInSecForIosWeb: 1,
            backgroundColor: Colors.black45,
            textColor: Colors.white,
            fontSize: 16.0
        );
    },
)
```
