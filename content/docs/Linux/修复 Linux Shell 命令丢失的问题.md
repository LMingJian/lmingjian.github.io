---
title: 修复 Linux Shell 命令丢失的问题
date: 2024-12-25T13:56:46+08:00
author: LiangMingJian
---

# BUG 描述

在 Linux shell 中执行 ls 命令时，报错 `bash：ls command not found`。

# Resolution

由于环境变量 PATH 被错误修改，导致命令丢失。

```bash
# 命令行执行，恢复环境变量
export PATH=/bin:/usr/bin:$PATH
```
