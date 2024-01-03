---
title: 如何完成对滚动位置的保存和横向滚动
date: 2022-11-02
author: LM
---

## 1.需求

滚动条能在界面刷新后保存滚动位置，且滚动条支持横向滚动。

## 2.难点

由于滚动事件`scroll`不会冒泡 DOM，因此用户无法使用 `$('body').on('scroll', 'div.dxgvHSDC + div', function () { })`来绑定事件。因此滚动事件的绑定必须绑定在对应元素

## 2.实现：滚动位置的保存

滚动位置可以使用会话储存 session 来记录，使用以下代码记录滚动位置。

```javascript
    $('.nav-list').scroll(function() {
        sessionStorage.setItem('scrollTop-b', $('.nav-list').scrollTop());
    });

    if(sessionStorage.getItem('scrollTop-b') != "undefined"){
        $('.nav-list').scrollTop(sessionStorage.getItem('scrollTop-b'));
    }
```

## 3.实现：横向滚动

```javascript
let container = document.querySelector(".container");
container.addEventListener("wheel", (event) => {
  event.preventDefault();   # 阻止默认的滚动事件
  container.scrollLeft += event.deltaY;   # 横向滚动
});
```

