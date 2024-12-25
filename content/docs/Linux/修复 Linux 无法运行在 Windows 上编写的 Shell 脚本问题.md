---
title: 修复 Linux 无法运行在 Windows 上编写的 Shell 脚本问题
date: 2024-12-25T13:56:36+08:00
author: LiangMingJian
---

# BUG 描述

Windows 下使用记事本编写的 shell 脚本，在上传到 Linux 系统后，无法运行。

# Resolution

这是由于 Windows 系统编码与 Linux 系统编码不同导致的，Windows 系统编码中回车会编码成 `\n\r`，而 Linux 系统中回车则是 `\n`，正因如此，Windows 下编写的 shell 脚本无法在 Linux 下运行。
