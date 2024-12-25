---
title: Web 的 Ajax 异步请求
date: 2024-12-25T16:17:07+08:00
author: LiangMingJian
---

# 概述

Ajax（Asynchronous Javascript And XML），即是异步的 JavaScript 和 XML。

Ajax 是浏览器与服务器之间的一种异步通信方式，它可以异步地向服务器发送请求，在等待响应的过程中，不会阻塞当前页面，在这种情况下，浏览器可以做自己的事情。直到成功获取响应后，浏览器才开始处理响应数据。

# 示例

```javascript
$.ajax({
     url: 'http://',
     type: 'post',
     dataType: 'json',
     data: {name: "x", foo: 'bar'},
     cache: false,
     headers: { 
         "Authorization": "Bearer token",
         'Content-Type': 'application/json',
     },                
     success: function(res){
         
     },
     error: function(e){
         
     },
});
```
