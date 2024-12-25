---
title: 如何在 Django 中设置 404 页面
date: 2024-12-25T11:07:27+08:00
author: LiangMingJian
---

# 404

在 Django 中，程序能自动的在`templates`目录下寻找名为`404.html`的文件，在路径错误时自动跳转并根据文件生成 404 界面。

需要注意的是，当 DEBUG = True 时，系统不会调用 404 文件。
