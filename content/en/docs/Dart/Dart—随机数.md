---
title: 随机数
date: 2021-07-23
author: LMingJian
---

## 1.随机数

在 dart 中，使用类 `Random()` 可以生成随机数。如下述代码所示。

```dart
import 'dart:math';
main(){
    // 实例化 Random 类并赋值给变量 rng；
    var rng = new Random();
    // 返回的 0~(max-1) 的随机数
    int x = rng.nextInt(int max);
    // 生成 1.0~0.0 的随机数
    double y = rng.nextDouble();
    // 生成随机布尔值: true/false
    bool z = rng.nextBool();
}
```

