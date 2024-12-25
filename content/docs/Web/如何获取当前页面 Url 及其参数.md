---
title: 如何获取当前页面 Url 及其参数
date: 2024-12-25T16:15:11+08:00
author: LiangMingJian
---

# 解决

```javascript
var url;
 
url = window.location.href; /* 获取完整URL */
console.log(url); /* http://127.0.0.1:8020/Test/index.html#test?name=test */
 
url = window.location.pathname; /* 获取文件路径(文件地址) */
console.log(url); /* /Test/index.html */
 
url = window.location.protocol; /* 获取协议 */
console.log(url); /* http */
 
url = window.location.host; /* 获取主机地址和端口号 */
console.log(url); /* http://127.0.0.1:8020/ */
 
url = window.location.hostname; /* 获取主机地址 */
console.log(url); /* http://127.0.0.1/ */
 
url = window.location.port; /* 获取端口号 */
console.log(url); /* 8020 */
 
url = window.location.hash; /* 获取锚点("#"后面的分段)*/
console.log(url); /* #test?name=test */
 
url = window.location.search; /* 获取属性("?"后面的分段)*/
console.log(url);
 
/* 获取请求参数的值 */
function GetQueryString(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    var r = window.location.search.substr(1).match(reg); //获取url中"?"符后的字符串并正则匹配
    var context = "";
    if (r != null){
        context = r[2];
    }
    reg = null;
    r = null;
    return context == null || context == "" || context == "undefined" ? "Null" : context;
}
```
