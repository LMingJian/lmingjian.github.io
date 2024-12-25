---
title: JS 函数的变量和变量提升
date: 2024-12-25T16:16:53+08:00
author: LiangMingJian
---

# 概述

在 JS 中，支持如下 3 种类型的变量。

- `var`：有变量提升，值可变，允许重复声明
- `let`：没有变量提升，值可变，不允许重复声明
- `const`：没有变量提升，值不可变，但如果是定义对象，则属性可变

# 变量提升

变量提升，又称声明提升，是指函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部，比如：

```javascript
x = 5; // 变量 x 设置为 5
console.log(x);
var x; // 声明 x
```

在运行时，var 变量 x 的声明被提升到最顶部。

```javascript
var x; // 声明 x
x = 5; // 变量 x 设置为 5
console.log(x);
```

需要注意的是，JavaScript 只有声明的变量会提升，初始化的不会。比如下述实例中的 y 便输出了 undefined，这是因为变量声明 var y 提升了，但是其初始化 y = 7并不会提升，所以 y 是一个未定义的变量。

```javascript
console.log(y); // undefined
var y = 7; // 初始化 y
```

上述代码等于。

```javascript
var y;
console.log(y);
y = 7;
```

另外，由于 let 不会变量提升，因此，我们如果将上述代码中的 var 变为 let，则情况就会变为下述样子。

```javascript
t = 2;
console.log(t);
let t;
// Cannot access 't' before initialization
console.log(t);
let t;
// t is not defined
```

# 使用 let 的好处

由于 let 不会变量提升，所以 let 可以解决诸如下述代码的问题。

```javascript
// 使用 var 变量
for(var i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  })
} // 5 5 5 5 5 

// 使用 let 变量
for(let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  })
} // 0 1 2 3 4
```

在上述代码中，由于 setTimeout 的使用，使得 i 的输出延迟，又因为 var 是函数作用域，会变量提升，所以上述 var 的代码类似于：

```javascript
var i
for(i = 0; i < 5; i++) {
  setTimeout(() => {})
} 
console.log(i) // 5 5 5 5 5 
```

在使用 var 变量时，由于变量提升，而导致的循环语句异常。

由于 let 是块作用域，不会变量提升，所以 let 的值仅作用于使用中括号`{}`包含的区域，如`for，while，if`语句，因此不会出现如 var 那样的问题。
