---
title: Flutter 读取应用资源
date: 2021-05-11
author: LM
---

## 1.资源加载

在 flutter 中，如果需要加载资源的话，需要在 pubspec.yaml 指定 APP 所需要的资源。这样的话，指定的每个 Asset （资源）都会被打包在 APP 中，并且在 APP 运行时可以访问到这些资源。

最常见的 Asset 类型就是图片，指定图片资源后即可以在 APP 页面使用图片控件加载资源了。

```yaml
# pubspec.yaml
flutter:
    assets:
        - assets/images/logo.png
```

```dart
// lib/main.dart
Image.asset('assets/images/logo.png')
```

## 2.使用 rootBundle 对象访问资源

APP 还可以通过引入 services 包使用 rootBundle 对象来访问资源。

```dart
import 'package:flutter/services.dart';
```

比如访问文件 test.txt，可以使用 rootBundle 对象的 loadString 方法。当然，前提也是需要在 pubspec.yaml 中指定资源才能访问的到。

```dart
rootBundle.loadString('assets/txt/test.txt').then((data){
    print(data);
});
```

因为 loadString() 返回的是 Future，所以需要用 then() 接受返回的 String 类型的数据。Future 类似于 ES6 中的 Promise，当异步任务执行完成后会把结果返回给 then()。

{{< details "参考文件" >}} 
1：[ Flutter 读取应用资源并显示  @飘香豆腐 ](https://zhuanlan.zhihu.com/p/243259521)
{{< /details >}}