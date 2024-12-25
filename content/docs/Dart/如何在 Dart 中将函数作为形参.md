---
title: 如何在 Dart 中将函数作为形参
date: 2024-12-25T11:05:45+08:00
author: LiangMingJian
---

# 将函数作为形参传递

在 Dart 中，允许将函数作为形参传递，在另一个函数内容进行调用，但需注意以下要求。

- 方法当做参数传递的时候，只需要传递方法名即可，不需要带上方法的括号。
- 方法作为参数的时候传递给其他方法的时候，不会立即执行。
- 方法当做参数传递的时候，方法名表示该方法的引用，这个引用当做参数传递的时候不会立即执行，只会在调用的时候执行。
- 入参方法在被实际调用时，会添加括号，当做正常的方法调用。

# 示例

```dart
int add(int a, int b) {
  return a + b;
}

int test(int a, int b, Function operation) {
  return operation(a, b);
}

main() {
  print(test(5, 2, add));
}
```
