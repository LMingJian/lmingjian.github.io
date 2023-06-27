---
title: JavaScript 运算符 ?? 与 ??=
date: 2022-08-22
author: LM
---

## 1.简介

在一些编程语言如 JavaScript 的代码中，支持如`??`，`??=`和`?.`这样的运算符。这些运算符通常应用在赋值语句中，来避免空值被赋给变量。

## 2.空值合并运算符（ ?? ）

如下述代码，**b 等于 a，如果 a 为 undefined、null，则 b 等于 c**

```javascript
let b
let a = 0
let c = { name:'XXX' }

b = a ?? c
```

## 3.空值赋值运算符（ ??= ）

如下述代码，**当 `??=` 左侧的值为 null、undefined 时，会将右侧变量的值赋给左侧变量。反之，如果右侧为空，则左侧的值不进行处理**。

```javascript
let b = '你好'
let a = 0
let c = null
let d = '123'
b ??= a;  // b = '你好'
c ??= d  // c = '123'
```

## 4.可选链（ ?. ）

如下述代码，**只有当 a 存在，且 a 具有 name 属性时，才会把值赋给 b ，否则 b 为 undefined**

```javascript
let a
let b = a?.name
```
