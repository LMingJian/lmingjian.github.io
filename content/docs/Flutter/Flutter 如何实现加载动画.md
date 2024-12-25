---
title: Flutter 如何实现加载动画
date: 2024-12-25T13:46:10+08:00
author: LiangMingJian
---

# 根据情况返回不同布局

要实现加载动画，首先需要在加载的时候返回加载的布局，不加载的时候返回登陆页面布局。

```dart
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  .........
}

class MyHomePage extends StatefulWidget {
  .........
}

class _MyHomePageState extends State<MyHomePage> {
  bool _loading = false; //标志！是否加载状态

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: _childLayout(),
    );
  }

  Widget _childLayout() {
    if (_loading) {
      return Center(
        child: Container(
          child: CircularProgressIndicator(),
        ),
      );
    } else {
      return Center(
        child: RaisedButton(
          onPressed: () { _loading = !_loading; }
          child: Text("显示加载动画"),
        ),
      );
    }
  }

}
```

# 使用 Stack 层叠布局

在原本布局上面叠加一层半透明背景，显示一个进度条。层叠布局至少有两个控件，自定义一个控件叫`ProgressDialog`，这个控件接收两个必传参数：子布局`child`，是否显示加载进度`loading`。

```dart
class ProgressDialog extends StatelessWidget {
  final bool loading;
  final Widget child;

  ProgressDialog({Key key, @required this.loading, @required this.child})
      : assert(child != null),
        assert(loading != null),
        super(key: key);

  @override
  Widget build(BuildContext context) {
    List<Widget> widgetList = [];
    widgetList.add(child);
    //如果正在加载，则显示加载添加加载中布局
    if (loading) {
      widgetList.add(Center(
        child: CircularProgressIndicator(),
      ));
    }
    return Stack(
      children: widgetList,
    );
  }
}
```

# 绘制透明效果

使用控件`Opacity`。

```dart
class ProgressDialog extends StatelessWidget {
  final bool loading;
  final Widget child;

  ProgressDialog({Key key, @required this.loading, @required this.child})
      : assert(child != null),
        assert(loading != null),
        super(key: key);

  @override
  Widget build(BuildContext context) {
    List<Widget> widgetList = [];
    widgetList.add(child);
    //如果正在加载，则显示加载添加加载中布局
    if (loading) {
      //增加一层黑色背景透明度为0.8
      widgetList.add(
        Opacity(
            opacity: 0.8,
            child: ModalBarrier(
              color: Colors.black87,
            )),
      );
      //环形进度条
      widgetList.add(Center(
        child: CircularProgressIndicator(),
      ));
    }
    return Stack(
      children: widgetList,
    );
  }
}
```

{{< details "参考文件" >}} 
1：[ Flutter做一个加载动画  @limhGeek ](https://juejin.cn/post/6844903744656637960)
{{< /details >}}
