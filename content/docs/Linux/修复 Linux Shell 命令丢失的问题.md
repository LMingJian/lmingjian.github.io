---
title: 修复 Linux Shell 命令丢失的问题
date: 2024-12-25T13:56:46+08:00
author: LiangMingJian
---

# BUG

在 Linux shell 中执行 ls 命令时，报错 `bash：ls command not found`。

# Resolution

由于系统的环境变量 PATH 被错误修改，导致的命令丢失，执行以下命令修复即可：

```bash
export PATH=/bin:/usr/bin:$PATH
```
