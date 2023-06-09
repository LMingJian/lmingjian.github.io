---
title: Pycharm 中无法执行脚本
date: 2023-03-16
author: LMingJian
---

## BUG 描述

新装系统后，Pycharm 启动终端执行命令如 pip 时报错，提示系统禁止执行脚本。

## Resolution

Windows 策略禁止运行没有数字签名的脚本，需要修改配置为 remotesigned 模式。

使用管理员身份开启 PowerShell ，输入以下命令执行。

```shell
Set-ExecutionPolicy RemoteSigned
```

