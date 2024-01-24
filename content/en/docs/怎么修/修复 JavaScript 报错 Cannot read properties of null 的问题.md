---
title: 修复 JavaScript 报错 Cannot read properties of null 的问题
date: 2022-12-05
author: LMingJian
---

## BUG 描述

浏览器控制台出现报错 `Cannot read properties of null (reading 'addEventListener')`。

## Resolution

因为 JavaScript 中操作 DOM 元素的函数方法需要在 HTML 文档渲染完成后才可以使用，如果没有渲染完成，此时的 DOM 树是不完整的，这样在调用一些 JavaScript 代码时就可能报出 undefined 错误。

要保证元素不会被读取错误，可以将绑定代码写在 `window.οnlοad=function(){...}` 里，或 `$(document).ready()` 里。

![load 和 ready 的区别](/images/drawingbed/img/202212051957191.png)

{{< details "参考文件" >}} 
1：[ 报错 Cannot read properties of null (reading 'addEventListener')_@zhangty0808 ](https://blog.csdn.net/qq_42101569/article/details/126431123)
{{< /details >}}