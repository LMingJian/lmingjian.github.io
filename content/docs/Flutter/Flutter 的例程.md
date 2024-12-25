---
title: Flutter 的例程
date: 2024-12-25T11:21:36+08:00
author: LiangMingJian
---

# 示例代码

下面是一个基本的 Flutter 应用例程，文章的后续会针对例程的每个部分进行分析。

```dart
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Flutter Demo",
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: "Flutter Demo Home Page"),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              "You have pushed the button this many times:",
            ),
            Text(
              "$_counter",
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: "Increment",
        child: Icon(Icons.add),
      ), 
    );
  }
}

```

# 代码分析

## 1）应用结构

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      //应用名称  
      title: "Flutter Demo", 
      theme: new ThemeData(
        //蓝色主题  
        primarySwatch: Colors.blue,
      ),
      //应用首页路由  
      home: new MyHomePage(title: "Flutter Demo Home Page"),
    );
  }
}
```

- MyApp 类代表 Flutter 应用，它继承自 StatelessWidget 类。
- 在 Flutter 中，大多数东西都是以 widget 的形式提供，包括对齐 alignment、填充 padding 和布局 layout 等。
- Flutter 在构建页面时，会调用组件的 build 方法，widget 的主要工作是提供一个 build 方法来描述如何构建 UI 界面（通常是通过组合、拼装其它基础 widget）。
- MaterialApp 是 Material 库中提供的 Flutter APP 框架，通过它可以设置应用的名称、主题、语言、首页及路由列表等。MaterialApp 也是一个 widget。
- home 为 Flutter 应用的首页，它也是一个 widget。

## 2）首页

```dart
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
 ...
}
```

- MyHomePage 是 Flutter 应用的首页，它继承自 StatefulWidget 类，表示它是一个有状态的组件。
- \_MyHomePageState 类是 MyHomePage 类对应的状态类。和 MyApp 类不同， MyHomePage 类中并没有 build 方法，取而代之的是，build 方法被挪到了 MyHomePage 类对应的状态类中。这样做是使得类的构建和 UI 界面的构建进行分离。

## 3）界面构建

```dart
 Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Text(
              "You have pushed the button this many times:",
            ),
            new Text(
              "$_counter",
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: "Increment",
        child: new Icon(Icons.add),
      ),
    );
  }
```

- Scaffold 是 Material 库中提供的页面脚手架，它提供了默认的导航栏、标题和包含主屏幕 widget 树的 body 属性。
- body 的组件树中包含了一个 Center 组件，Center 可以将其子组件树对齐到屏幕中心。此例中， Center 子组件是一个 Column 组件，Column 的作用是将其所有子组件沿屏幕垂直方向依次排列； 此例中 Column 子组件是两个 Text，第一个Text 显示固定文本 You have pushed the button this many times，第二个 Text 显示 \_counter 状态的数值。
- floatingActionButton 是出现在页面右下角的带 + 的悬浮按钮，它的 onPressed 属性接受一个回调函数，代表它被点击后的处理器，本例中将 \_incrementCounter 方法作为其处理函数。

{{< details "参考文件" >}} 
1：[《Flutter实战》@wendux ](https://book.flutterchina.club/)
{{< /details >}}
