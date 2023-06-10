---
title: 数据类型：整型和浮点型
date: 2021-04-24
author: LMingJian
---

## 1.整型

在 Dart 中，使用`int`表示整型，使用`double`表示浮点型。

### a.属性

```dart
int figureA = -93;
// figureA 是否为负数
print(figureA.isNegative);
// figureA 是否是有限的
print(figureA.isFinite);
// figureA 是否正无穷大或负无穷大
print(figureA.isInfinite);

int figureC = 13;
// figureC 是否为奇数
print(figureC.isOdd);
// figureC 是否为偶数
print(figureC.isEven);
// 返回 figureC 所占存储位
print(figureC.bitLength);

double figureB = 64.742;
// 返回 figureB 的符号，-1.0:值小于0、+1.0:值大于0、-0.0/0.0/NaN:值是其本身
print(figureB.sign);
// 返回 figureB 运行时的类型
print(figureB.runtimeType);
// 返回 figureB 的哈希码
print(figureB.hashCode);
```

### b.方法

```dart
int figureA = -93;
// 返回 figureA 的绝对值
print(figureA.abs());
// 返回 figureA 的字符串
print(figureA.toString());

int figureC = 31;
// figureC 对比其他整数，0:相同、1:大于、-1:小于
print(figureC.compareTo(20));
// 返回 figureC 转换成指定基数(进制)的字符串
print(figureC.toRadixString(16));
// 返回 figureC 与其他整数的最大公约数
print(figureD.gcd(18));
// 返回 figureC 与其他整数的截取余数
print(figureD.remainder(18));
// 返回 figureC 几次幂值的字符串
print(figureD.toStringAsExponential(2));

double figureB = 64.742;
// 返回 figureB 的整数值
print(figureB.toInt());
// 返回 figureB 的双精度值
print(figureB.toDouble());
// 返回大于 figureB 的双精度值
print(figureB.ceilToDouble());
// 返回小于 figureB 的双精度值
print(figureB.floorToDouble());
// 返回 figureB 四舍五入的双精度值
print(figureB.roundToDouble());
// 返回 figureB 保留几位小数的字符串
print(figureB.toStringAsFixed(2));
// 返回 figureB 保留几位小数后精确结果的字符串
print(figureB.toStringAsPrecision(3));
```
