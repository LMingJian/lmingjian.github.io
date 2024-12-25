---
title: JS 的循环
date: 2024-12-25T16:16:46+08:00
author: LiangMingJian
---

# for of  和  for  in

- for in ：以任意顺序遍历一个对象的除 Symbol 以外的可枚举属性。
- for of ：在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。

for in 与 for of 的区别在于它们的迭代方式。

- for in 语句以任意顺序迭代对象的可枚举属性。
- for of 语句遍历可迭代对象定义要迭代的数据。

比如：

```javascript
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';
console.log(iterable)
// [3, 5, 7, foo: 'hello']

for (let i in iterable) {
  console.log(i); 
  // 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); 
  // 3, 5, 7
}
```

上述代码中每个对象 Object 都将继承 objCustom 的属性，并且作为 Array 的每个对象要继承 arrCustom 属性，因此对象 iterable 继承属性 objCustom 和 arrCustom。

在使用 for in 迭代数据时，循环打印的是 iterable 对象的所有可枚举属性，它不打印数组元素 3， 5，7 或 hello，因为这些不是枚举属性，它**打印了数组的索引以及 arrCustom 和 objCustom**。

```javascript
for (let i in iterable) {
  console.log(i); // 0, 1, 2, "foo", "arrCustom", "objCustom"
}
```

在使用 for of 迭代数据时，循环**打印的是 iterable 作为可迭代对象定义的迭代值，即数组元素 3，5，7，不包括继承下来 objCustom 和 arrCustom 的值**。

```javascript
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```
