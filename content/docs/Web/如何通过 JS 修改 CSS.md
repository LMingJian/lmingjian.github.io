---
title: 如何通过 JS 修改 CSS
date: 2024-12-25T16:15:43+08:00
author: LiangMingJian
---

# 需求

通过 JavaScript 动态的修改元素 CSS 样式。

# 实现

1.通过 `style.cssText` 修改：

`div.style.cssText='width:250px;height:250px;';`

2.通过 `style.setProperty()` 修改：

`div.style.setProperty('width','250px');`

3.通过 `style.property` 修改：

`div.style.width = "250px";`
