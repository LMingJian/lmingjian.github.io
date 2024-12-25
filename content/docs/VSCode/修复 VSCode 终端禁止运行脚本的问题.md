---
title: 修复 VSCode 终端禁止运行脚本的问题
date: 2024-12-25T16:14:42+08:00
author: LiangMingJian
---

# BUG 描述

VSCode 终端出现报错：`node_modules/.bin/babel : 无法加载文件 D:\node\node_project\es6\node_modules\.bin\babel.ps1，因为在此系统上禁止运行脚本...`。

# Resolution

这是由于 Windows 系统不允许在不被信任的环境下运行脚本，需要重新配置权限解决该问题。

- 以管理员身份运行 VSCode
- 执行：get-ExecutionPolicy，此时显示 Restricted，表示状态是禁止的
- 执行：set-ExecutionPolicy RemoteSigned
- 再执行 get-ExecutionPolicy，显示 RemoteSigned，表示状态启用
