---
title: 如何完成对鼠标点击位置的定位
date: 2021-06-05
author: LM
---

## 1.需求

在用户点击网页后，代码能定位到点击位置。

## 2.实现

```javascript
$('body').click(function(even) { 
    // 在页面任意位置点击而触发此事件
    // even.target 表示被点击的目标
    console.log($(even.target).attr("id"))
})
```

