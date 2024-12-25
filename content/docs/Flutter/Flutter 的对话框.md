---
title: Flutter 的对话框
date: 2024-12-25T11:21:30+08:00
author: LiangMingJian
---

# showDialog

showDialog 用于弹出普通 Material 风格的对话框，用法如下：

```dart
showDialog(
    context: context,
    builder: (context) {
      return AlertDialog(
        ...
      );
    }
);
```

# showCupertinoDialog

showCupertinoDialog 用于弹出 ios 风格对话框，基本用法如下：

```dart
showCupertinoDialog(
    context: context,
    builder: (context) {
      return CupertinoAlertDialog(
       ...
      );
    });
```

# showAboutDialog

AboutDialog 用于描述当前 App 信息，底部提供 2 个按钮：查看许可按钮和关闭按钮。AboutDialog 需要和 showAboutDialog 配合使用，用法如下：

```dart
showAboutDialog(
  context: context,
  applicationIcon: Image.asset(
    'images/bird.png',
    height: 100,
    width: 100,
  ),
  applicationName: '应用程序',
  applicationVersion: '1.0.0',
  applicationLegalese: 'copyright 老孟，一枚有态度的程序员',
  children: <Widget>[
    Container(
      height: 30,
      color: Colors.red,
    ),
    Container(
      height: 30,
      color: Colors.blue,
    ),
    Container(
      height: 30,
      color: Colors.green,
    )
  ],
);
```

# showModalBottomSheet

从底部弹出，通常和 BottomSheet 配合使用，用法如下：

```dart
showModalBottomSheet(
        context: context,
        builder: (BuildContext context) {
          return BottomSheet(...);
        });
```

# showCupertinoModalPopup

showCupertinoModalPopup 展示 ios 的风格弹出框，通常情况下和 CupertinoActionSheet 配合使用，用法如下：

```dart
showCupertinoModalPopup(
    context: context,
    builder: (BuildContext context) {
      return CupertinoActionSheet(
        title: Text('提示'),
        message: Text('是否要删除当前项？'),
        actions: <Widget>[
          CupertinoActionSheetAction(
            child: Text('删除'),
            onPressed: () {},
            isDefaultAction: true,
          ),
          CupertinoActionSheetAction(
            child: Text('暂时不删'),
            onPressed: () {},
            isDestructiveAction: true,
          ),
        ],
      );
    }
);
```

# 其他

```dart
showMenu()
showSearch()
```

{{< details "参考文件" >}} 
1：[ Flutter内置show @老孟Flutter ](https://www.imooc.com/article/302188)
{{< /details >}}
