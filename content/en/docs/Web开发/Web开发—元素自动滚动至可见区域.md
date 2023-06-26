---
title: 网页开发：使元素自动滚动至可见区域
date: 2022-01-24
author: LM
---

## 1.需求

让网页自动将元素滚动至可见区域。

## 2.实现

`scrollIntoView()`方法可以将调用它的元素滚动到浏览器窗口的可见区域。

```javascript
var element = document.getElementById("box");
 
element.scrollIntoView();
element.scrollIntoView(false);
element.scrollIntoView({block: "end"});
element.scrollIntoView({behavior: "instant", block: "end", inline: "nearest"});
```

