---
title: 什么是 UrlEncode
date: 2024-12-25T10:34:29+08:00
author: LiangMingJian
---

# 概述

在网络访问过程中，我们常常在复制网址的时候可以看见这样的内容：`https://cn.bing.com/search?q=%E8%B5%B7%E7%82%B9`，这个内容在浏览器中实际表示的是 `https://cn.bing.com/search?q=起点`，为什么会出现这种状况？

这是因为在网络传输过程中，URL（统一资源定位符，网址）中的某些字符（如空格、特殊符号、中文字符等）可能无法传输，或者被浏览器或服务器误解，容易导致链接失效或数据错误。因此，浏览器会自动的将这些字符进行编码转换，确保信息在传输过程中不被篡改或丢失。

上述的这种编码转换使用的就是 UrlEncode（URL 编码）。

# 什么是 UrlEncode

UrlEncode 是一种将 URL 中的特殊字符转换为特定格式字符的编码方式。

UrlEncode 通过将字符转换为 ASCII 码的十六进制表示，并在前面加上百分号（%）来标识，使得所有不符合规格的字符变更为 %+ASCII 的格式。

UrlEncode 的使用确保了确保 URL 中的特殊字符能被所有浏览器和服务器正确解析；防止恶意字符注入攻击，如 SQL 注入；以及统一 URL 的表示形式，避免因地区字符差异导致的链接失效。

# UrlEncode 的编码规则

UrlEncode 的转换遵循以下的规则：

‌**字母和数字**：直接保留，无需二次编码。

‌**特殊字符**：`%` 后跟对应字符 ASCII 码的两位十六进制数。

| 符号 | 空格 | !    | "    | #    | $    | %    | 
| ---- | ---- | ---- | ----| ---- | ---- | ---- |
| URL 编码  | %20  | %21  | %22  | %23  | %24  | %25  |
| ASCII 码  | 20  | 21  | 22 | 23  | 24  | 25  |

> ASCII 码对照可查看 [ ASCII码对照表 ](https://c.biancheng.net/c/ascii/)

**非 ASCII 字符**‌（如中文）：先转换为 UTF-8 字节流序列，然后在每一个字节前添加 `%`。

比如，`你好` 以 UTF-8 转换后，字节流有 `E4BDA0E5A5BD`，对应 16 进制有 `\xE4\xBD\xA0\xE5\xA5\xBD`，以每两个字符为一个字节，在字节前添加 `%` 完成 UrlEncode 有 `%E4%BD%A0%E5%A5%BD`。

# JavaScript UrlEncode 编解码实现

```JavaScript
// URL 编码
function urlEncode(str) {
    return encodeURIComponent(str);
}

// URL 解码
function urlDecode(str) {
    return decodeURIComponent(str);
}
```

# Python UrlEncode 编解码实现

```python
# URL 编码
def url_encode(text):
    return urllib.parse.quote(text, safe=':/?=&')

# URL 解码
def url_decode(text):
    return urllib.parse.unquote(text)
```
