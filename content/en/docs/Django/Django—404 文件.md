---
title: Django 的 404 界面的设置
date: 2021-08-26
author: LM
---

## 1.Django 的 404 界面

在 Django 中，程序能自动的在`templates`目录下寻找名为`404.html`的文件，在路径错误时自动跳转并根据文件生成 404 界面。

需要注意的是，当 DEBUG = True 时，系统不会调用 404 文件。

