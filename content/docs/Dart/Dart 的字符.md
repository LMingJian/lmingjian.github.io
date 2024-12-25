---
title: Dart 的字符
date: 2024-12-25T11:06:20+08:00
author: LiangMingJian
---

# 声明

在 Dart 中，字符有 `String` 和 `StringBuffer`。`String` 是字符常量，必须在初始化时赋值。 `StringBuffer`是字符变量，可以不进行初始化赋值，而是在后续使用中写入值。

# String 字符常量

## 支持的属性

```dart
String str = "Hello world!";
// 返回字符串的 UTF-16 代码单元列表
print(str.codeUnits);
// 返回根据代码单元生成的哈希码
print(str.hashCode);
// 字符串是否为空
print(str.isEmpty);
// 字符串是否不为空
print(str.isNotEmpty);
// 字符串的长度
print(str.length);
// 返回字符串 Unicode 代码的可迭代对象
print(str.runes);
// 返回对象运行时的类型
print(str.runtimeType);
```

## 支持的方法

```dart
// 返回对象的字符串表示
String str = "Hello world!";
print(str.toString());

// 截取字符串
String str = 'Dart is fun';
String newStr = str.substring(0,4);
print(newStr);

// 在字符串中插入字符串
String name = "XiaoMing";
print("My name is ${name}");

// 输出字符串的Unicode编码
String str = "Dart";
print(str.codeUnitAt(0));
print(str.codeUnits);

// 去掉字符串前后空格
String str = "\tDart is fun\n";
print(str.trimLeft());
print(str.trimRight());
print(str.trim());

// 字符串的大小写转换
String str = "ABCdef";
print(str.toLowerCase());
print(str.toUpperCase());

// 拆分字符串
String strA = "Hello world!";
print(strA.split(" "));
String strB = "abba";
print(strB.split(new RegExp(r"b*")));

// 是否包含其他字符串
String str = 'Dart strings';
print(str.contains('D'));
print(str.contains(new RegExp(r'[A-Z]')));
print(str.contains('D',0));
print(str.contains(new RegExp(r'[A-Z]'),0));

// 在字符串前后补占位符
String str = "86";
print(str.padLeft(4,'0'));
print(str.padRight(4,'0'));

// 获取指定字符出现的位置
String str = 'Dartisans';
print(str.indexOf('art'));
print(str.indexOf(new RegExp(r'[A-Z][a-z]')));
print(str.lastIndexOf('a'));
print(str.lastIndexOf(new RegExp(r'a(r|n)')));

// 替换字符串中所有匹配字符
String str = "resume";
print(str.replaceAll(new RegExp(r'e'),'é'));
```

# StringBuffer 字符变量

## 支持的属性

```dart
StringBuffer strBuf = new StringBuffer();
strBuf.write("Sow nothing,reap nothing.");
// 返回字符串缓冲区的哈希码
print(strBuf.hashCode);
// 字符串缓冲区是否为空
print(strBuf.isEmpty);
// 字符串缓冲区是否不为空
print(strBuf.isNotEmpty);
// 返回字符串缓冲区累积内容的长度
print(strBuf.length);
// 返回对象运行时的类型
print(runtimeType);
```

## 支持的方法

```dart
StringBuffer strBuf = new StringBuffer();
// 添加字符串到字符串缓冲区内
strBuf.write("Do one thing at a time,and do well.");
// 返回字符串缓冲区的所有内容
print(strBuf.toString());
// 清除字符串缓冲区
strBuf.clear();
```
