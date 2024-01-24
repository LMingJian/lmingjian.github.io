---
title: 修复 Flutter 公共类不允许有私有参数的问题
date: 2022-08-22
author: LM
---

## 1.问题

编写 Flutter 应用时，若 StatefulWidget 类中创建 _ExampleState 类，则编辑器会出现报错 LIBRARY_PRIVATE_TYPES_IN_PUBLIC_API，提示用户不要在公共函数或类中使用私有参数。

这是由于 Flutter 的编程规范出现变更：[ 不要在库的公开的API中使用私有类型（library_private_types_in_public_api） ](https://links.jianshu.com/go?to=https%3A%2F%2Fdart-lang.github.io%2Flinter%2Flints%2Flibrary_private_types_in_public_api.html)。

## 2.解决方案

将以下内容。

```dart
class Example extends StatefulWidget {
  const Example({Key? key}) : super(key: key);

  @override
  _ExampleState createState() => _ExampleState();
}

class _ExampleState extends State<Example> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

重写为。

```dart
class Example extends StatefulWidget {
  // you can also now use a super initializer for key 
  // if you are using dart 2.17
  const Example({super.key});

  // now returning State<Example>
  @override
  State<Example> createState() => _ExampleState();
}

class _ExampleState extends State<Example> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
```

{{< details "参考文件" >}} 
1：[ library_private_types_in_public_api and StatefulWidget-Flutter ](https://www.appsloveworld.com/flutter/100/10/library-private-types-in-public-api-and-statefulwidget)
{{< /details >}}