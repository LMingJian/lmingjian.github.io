---
title: 如何使用 jQuery 实现局部刷新
date: 2024-12-25T16:15:38+08:00
author: LiangMingJian
---

# 需求

网页部分内容更新后，能进行局部元素的刷新，而不是整个页面的刷新。

# 使用 load 方法实现局部刷新

在`jQuery`中，`load`方法可以加载本地`Html`文件里的某个元素，使用这个特性可以实现`Html`的局部更新。

```javascript
$("#content").load("list .table")
```

# 使用 location 实现全局刷新

```javascript
window.location.reload()
```
