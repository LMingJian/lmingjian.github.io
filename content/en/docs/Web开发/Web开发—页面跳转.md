---
title: 如何实现页面的跳转
date: 2020-12-05
author: LM
---

## 1.需求

点击链接后，网页自动进行跳转。

## 2.实现：原页面跳转

```html
<a href="https://www.baidu.com">BAT</a>
```

```javascript
window.location.href="https://www.baidu.com";
```

## 3.实现：打开新标签页

```html
<a href="https://www.baidu.com" target="_blank">BAT</a>
```

```javascript
window.open("https://www.baidu.com");
```