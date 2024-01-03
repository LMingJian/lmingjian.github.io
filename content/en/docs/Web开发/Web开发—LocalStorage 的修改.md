---
title: 如何修改 LocalStorage
date: 2021-01-16
author: LM
---

## 1.需求

修改 LocalStorage 和 SessionStorage 中的 token。

![](/images/drawingbed/img/202205051036044.png)

## 2.实现

使用 JavaScript 进行获取 token 和修改 token 的操作

```javascript
// 设置 localStorage
localStorage.setItem(name, val);
// 获取 localStorage
localStorage.getItem(name);
```
