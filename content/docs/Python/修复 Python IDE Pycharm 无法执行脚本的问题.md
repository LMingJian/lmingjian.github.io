---
title: 修复 Python IDE Pycharm 无法执行脚本的问题
date: 2024-12-25T16:10:53+08:00
author: LiangMingJian
---

# BUG 描述

新装系统后，Pycharm 启动终端执行命令如 pip 时报错，提示系统禁止执行脚本。

# Resolution

Windows 策略禁止运行没有数字签名的脚本，需要修改配置为 remotesigned 模式。

使用管理员身份开启 PowerShell ，输入以下命令执行。

```bash
Set-ExecutionPolicy RemoteSigned
```
