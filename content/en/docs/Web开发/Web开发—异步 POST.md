---
title: 网页开发：异步网络请求
date: 2021-07-24
author: LM
---

## 1.需求

异步网络请求。

## 2.实现：异步 Ajax

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

