---
title: 什么是 UrlEncode
date: 2024-12-25T10:34:29+08:00
author: LiangMingJian
---

# 什么是 UrlEncode

UrlEncode 是一个函数，作用是将字符串以 URL 编码，是特定上下文的统一资源定位符（URL）的编码机制，最终使 HTML 数据安全提交。

函数会将字符串以URL编码，例如空格编码为加号，常规网页中的表单数据传送就是用 UrlEncode 编码后再送出。

部分转换规则如下：

| 空格 | !    | #    | $    | %    | +    | @    | :    | =    | ?    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| %20  | %21  | %23  | %24  | %25  | %2B  | %40  | %3A  | %3D  | %3F  |

# 在 ASP

```
Server.URLEncode("内容")
```

# 在 PHP

```
urlencode("内容");
```

# 在 JSP

```
URLEncoder.encode("要转码的内容");
```

# 在 Python

```
urllib.parse.unquote("内容")
```
