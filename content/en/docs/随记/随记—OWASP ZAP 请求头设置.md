---
title: OWASP ZAP 的请求头设置
date: 2022-07-29
author: LM
---

## 1.问题

在测试时，已知 Token ，需要在每个发送的请求中添加以跳过验证。这时可以使用 OWASP ZAP 的 Replacer 功能实现 Token 的添加。

## 2.实现

在界面中按下 `Ctrl + R`，跳转至功能界面，寻找 Request Header 进行添加设置。在设置后，所填内容将被视为请求头。如果请求头存在，则其值将被替换为所填值。如果所填值为空，则请求头将被删除（如果存在）。如果请求头不存在且值不为空，则将添加请求头。

{{< details "参考文件" >}} 
1：[ OWASP ZAP – Replacer (zaproxy.org) ](https://www.zaproxy.org/docs/desktop/addons/replacer/#request-header-will-add-if-not-present)
{{< /details >}}