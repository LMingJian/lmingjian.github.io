---
title: Flutter 如何实现状态或数据监听
date: 2024-12-25T13:46:15+08:00
author: LiangMingJian
---

# ValueListenableBuilder

在`Widget`中配合`ValueListenableBuilder`，`ValueNotifier`实现对数据的监听。

```dart
/// 定义 ValueNotifier 这里传递的数据类型为 String
ValueNotifier<String> _testValueNotifier = ValueNotifier<String>('');
/// 定义数据变化后监听的 Widget
Widget buildValueListenableBuilder() {
  return ValueListenableBuilder(
    /// 数据发生变化时回调,变化的布局
    builder: (context, value, child) {
        if(value == ''){
          return Text('空');
        }else{
          return Text(value);
        }
    },
    /// 监听的数据
    valueListenable: _testValueNotifier,
  );
}
/// 数据变化
 void testFunction() {
   /// 赋值更新
   _testValueNotifier.value = '传递的测试数据';
 }
```
