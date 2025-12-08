---
title: 如何设置 OWASP ZAP 的请求头
date: 2024-12-25T10:17:39+08:00
author: LiangMingJian
---

# 需求

在测试时，已知 Token ，需要在每个发送的请求中添加以跳过验证。

这时可以使用 OWASP ZAP 的 Replacer 功能实现 Token 的添加。

# 实现

在界面中按下 `Ctrl + R`，跳转至功能界面，寻找 Request Header 进行添加设置。

在设置后，所填内容将被视为请求头。如果请求头存在，则其值将被替换为所填值。如果所填值为空，则请求头将被删除。如果请求头不存在且值不为空，则将添加请求头。

————————————

> [ OWASP ZAP – Replacer (zaproxy.org) ](https://www.zaproxy.org/docs/desktop/addons/replacer/#request-header-will-add-if-not-present)
