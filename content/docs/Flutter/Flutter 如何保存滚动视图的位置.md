---
title: Flutter 如何保存滚动视图的位置
date: 2024-12-25T11:21:57+08:00
author: LiangMingJian
---

# 使用 AutomaticKeepAliveClientMixin 保存状态

通过在`Widget`的`State`中混入`AutomaticKeepAliveClientMixin`，再重写`wantKeepAlive`的值为`true`，这样来保留列表项的状态。

```dart
class GetListView extends StatefulWidget{
  @override
  State<StatefulWidget> createState() =>_GetListViewState();
}

class _GetListViewState extends State<GetListView> with AutomaticKeepAliveClientMixin<GetListView>{

  @override
  Widget build(BuildContext context){
    return ListView.builder(
        itemCount: 2000,
        itemBuilder: (context, i) {
            return ListTile(
                title: Text(
                    i.toString(),
                    textScaleFactor: 1.5,
                    style: TextStyle(color: Colors.blue),
                ));
        });
  }

  @override
  bool get wantKeepAlive => true;

} 
```

# 使用 PageStorageKey 保存偏移

通过在列表`builder`中使用`PageStorageKey`手动保存偏移值，这样来保留列表项的状态。

```dart
ListView.builder(
    key: PageStorageKey<String>("controllerA"),
    controller: ScrollController(keepScrollOffset: true),
    itemCount: 2000,
    itemBuilder: (context, i) {
        print("Rebuilded 1");
        return ListTile(
            title: Text(
                i.toString(),
                textScaleFactor: 1.5,
                style: TextStyle(color: Colors.blue),
            ));
    }),
```

{{< details "参考文件" >}} 
1：[ how-to-get-flutter-scrollcontroller-to-save @stackoverflow ](https://stackoverflow.com/questions/60292911/how-to-get-flutter-scrollcontroller-to-save-position-of-listview-builder-when)
{{< /details >}}
