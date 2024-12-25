---
title: 修复 Selenium 使用无头浏览器时，部分内容无法加载的问题
date: 2024-12-25T16:13:37+08:00
author: LiangMingJian
---

# BUG 描述

在运行 `selenium` 进行自动化时，有一些页面无法 100% 加载。

# Resolution

排除了网络问题导致元素未加载的原因后，问题仍然存在，不清楚究竟是什么原因，但可以通过使用 FireFox webdriver （geckodriver）来解决该问题。
