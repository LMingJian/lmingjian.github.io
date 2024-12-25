---
title: 如何修改浏览器的 LocalStorage
date: 2024-12-25T16:15:46+08:00
author: LiangMingJian
---

## 1.需求

修改 LocalStorage 和 SessionStorage 中的 token。

![](/_images/drawingbed/img/202205051036044.png)

## 2.实现

使用 JavaScript 进行获取 token 和修改 token 的操作

```javascript
// 设置 localStorage
localStorage.setItem(name, val);
// 获取 localStorage
localStorage.getItem(name);
```
