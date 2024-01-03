---
title: 如何使用 jQuery 实现局部刷新
date: 2021-07-23
author: LM
---

## 1.需求

在网页部分内容内容更新后，局部的刷新界面。

## 2.实现：使用 Load 

在`jQuery`中`load`方法可以加载本地`Html`文件里的某个元素，使用这个特性可以实现`Html`的局部更新

```javascript
$("#content").load("list .table")
```

## 3.实现：使用 location 实现全局刷新

```javascript
window.location.reload()
```

