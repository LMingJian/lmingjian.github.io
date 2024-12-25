---
title: JS 函数的默认形参和剩余形参
date: 2024-12-25T16:16:56+08:00
author: LiangMingJian
---

# 默认形参

在 ES6 后，用户可以在创建函数时设置包含默认值的形参。

```javascript
function fn (name = 'ABC', age = 18) {
  console.log(name, age)
}
fn() // ABC 18
fn('ABCD', 22) // ABCD 22
```

# 剩余形参

使用剩余形参，JS 允许我们将一个不定数量的参数表示为一个数组传入。

```javascript
function fn (name, ...params) {
  console.log(name)
  console.log(params)
}
fn ('ABCD', 1, 2) // ABCD [ 1, 2 ]
fn ('ABCD', 1, 2, 3, 4, 5) // ABCD [ 1, 2, 3, 4, 5 ]
```
