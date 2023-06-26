---
title: 网页开发：使用 JavaScript 修改 CSS
date: 2022-06-20
author: LM
---

## 1.需求

通过 JavaScript 修改元素的 CSS 样式。

## 2.实现

1. 通过 `style.cssText` 修改：`div.style.cssText='width:250px;height:250px;';`
2. 通过 `style.setProperty()` 修改：`div.style.setProperty('width','250px');`
3. 通过 `style.property` 修改：`div.style.width = "250px";`