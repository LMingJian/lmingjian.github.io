---
title: 修复 Wapiti 报告丢失 js 文件的问题
date: 2024-12-25T10:36:35+08:00
author: LiangMingJian
---

# BUG 描述

在 wapiti 的 3.0.4 版本中：`HTTP request and cURL command hidden on html report`，HTTP 报告缺少 js 文件。

# Resolution

官方 issues：`Ok, when generating the html report it is supposed to copy the js file from wapitiCore/report_template into output directory, that is why it was missing`。手动从目录 `wapitiCore/report_template` 中找到并添加到对应文件夹内。

{{< details "参考文件" >}} 
1：[ github-wapiti-issues @Maxime Alay-Eddine ](https://github.com/wapiti-scanner/wapiti/issues/86)
{{< /details >}}
