---
title: 修复 gitlab CI 无法运行无标签工作的问题
date: 2024-12-25T13:47:47+08:00
author: LiangMingJian
---

# BUG 描述

gitlab CI 无法执行没有标记标签的工作。

# Resolution

定位`gitlab 项目设置 -> CI/CD -> Runner`，找到需要执行的 runner，点击编辑按钮，启用 `Run untagged jobs`。
