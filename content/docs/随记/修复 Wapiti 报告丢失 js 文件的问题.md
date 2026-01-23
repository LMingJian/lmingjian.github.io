---
title: 修复 Wapiti 报告丢失 js 文件的问题
date: 2024-12-25T10:36:35+08:00
author: LiangMingJian
---

# BUG 描述

在 Wapiti 的 3.0.4 版本中，HTML 报告出现报错：`HTTP request and cURL command hidden on html report`，提示 HTML 缺少 js 文件。

# Resolution

根据官方 issues 描述可知，这是因为该版本打包的文件出现异常导致，需要用户手动从目录 `wapitiCore/report_template` 中找到 js 文件并添加到对应文件夹内。

```
Ok, when generating the html report it is supposed to copy the js file from wapitiCore/report_template into output directory, that is why it was missing
```

> 1：[ github-wapiti-issues @Maxime Alay-Eddine ](https://github.com/wapiti-scanner/wapiti/issues/86)
