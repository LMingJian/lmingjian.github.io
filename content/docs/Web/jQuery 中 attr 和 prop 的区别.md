---
title: jQuery 中 attr 和 prop 的区别
date: 2024-12-25T16:16:17+08:00
author: LiangMingJian
---

# 概述

在 jQuery 中，`attr()` 和 `prop()` 都表示"属性"的意思，都能获取属性值，但是两者操作的对象不同。

# 区别

- `attr()` 操作的是文档节点的属性，因此设置的属性值只能是字符串类型，如果不是字符串类型，也会调用其 `toString()` 方法，将其转为字符串类型。
- `prop()` 操作的是 JS 对象的属性，因此设置的属性值可以为包括数组和对象在内的任意类型。

比如，对于表单元素的 checked、selected、disabled 等属性：

- 使用 `attr()` 获取这些属性的返回值为 String 类型，如果被选中(或禁用)就返回checked、selected 或 disabled，否则(即元素节点没有该属性)返回 undefined。
- 使用 `prop()` 则返回 bool 值的 true 与 false 以表示该属性实时状态。
