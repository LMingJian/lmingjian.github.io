---
title: 如何进行 Url 编解码
date: 2024-12-25T16:15:17+08:00
author: LiangMingJian
---

# 需求

在网页异步请求时，完成对中文的 URL 编码，解码。

#  使用 escape 实现

`escape()` 函数可对字符串进行编码，以便在所有的计算机上传输，读取字符串数据。该方法不会对 ASCII 字母和数字进行编码，也不会对 `* @ - _ + . /` 这些 ASCII 标点符号进行编码 。

可以使用 `unescape()` 函数对字符串进行解码。

PS：`escape()` 函数已经从 Web 标准中删除，请尽量不使用该函数。

```javascript
console.log(escape("春节+国庆"))
// %u6625%u8282+%u56FD%u5E86
console.log(escape("春节=+=国庆"))
// %u6625%u8282%3D+%3D%u56FD%u5E86
console.log(unescape('%u6625%u8282%3D+%3D%u56FD%u5E86'))
// 春节=+=国庆
```

# 使用 encodeURI 实现

`encodeURI()` 函数可把字符串作为 URI 进行编码。对如 `, / ? : @ & = + $ #` 的 ASCII 标点符号，`encodeURI()` 函数不会进行转义。

可以使用 `decodeURI()` 方法进行解码。

```javascript
console.log(encodeURI('http://baidu.com?hello=您好&word=文档'))
// http://baidu.com?hello=%E6%82%A8%E5%A5%BD&word=%E6%96%87%E6%A1%A3
console.log(decodeURI('http://baidu.com?hello=%E6%82%A8%E5%A5%BD&word=%E6%96%87%E6%A1%A3'))
// http://baidu.com?hello=您好&word=文档
```

## 使用 encodeURIComponent 实现

`encodeURIComponent()` 能编码如 `; / ? : @ & = + $ , #` 这些特殊字符。

可以使用 `decodeURIComponent()` 进行解码。

```javascript
console.log(encodeURIComponent('http://baidu.com?hello=您好&word=文档'))
// http%3A%2F%2Fbaidu.com%3Fhello%3D%E6%82%A8%E5%A5%BD%26word%3D%E6%96%87%E6%A1%A3
console.log(decodeURIComponent('http%3A%2F%2Fbaidu.com%3Fhello%3D%E6%82%A8%E5%A5%BD%26word%3D%E6%96%87%E6%A1%A3'))
// http://baidu.com?hello=您好&word=文档
```
