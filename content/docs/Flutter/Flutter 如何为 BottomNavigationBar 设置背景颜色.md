---
title: Flutter 如何为 BottomNavigationBar 设置背景颜色
date: 2024-12-25T13:46:30+08:00
author: LiangMingJian
---

# BottomNavigationBar

BottomNavigationBar 是 Flutter 提供的底部工具栏组件，其不能直接设置背景色，但是可以通过设置主题画布色达到相同的效果。

```dart
  bottomNavigationBar: new Theme(
    data: Theme.of(context).copyWith(
        //设置背景色`BottomNavigationBar`
        canvasColor: Colors.green,
        //设置高亮文字颜色
        primaryColor: Colors.red,
        //设置一般文字颜色
        textTheme: Theme.of(context).textTheme.copyWith(caption: new TextStyle(color: Colors.yellow))), 
    child: new BottomNavigationBar(
      type: BottomNavigationBarType.fixed,
      currentIndex: 0,
      items: [
        new BottomNavigationBarItem(
          icon: new Icon(Icons.add),
          title: new Text("新增"),
        ),
        new BottomNavigationBarItem(
          icon: new Icon(Icons.delete),
          title: new Text("删除"),
        )
      ],
    ),
  ),
```
