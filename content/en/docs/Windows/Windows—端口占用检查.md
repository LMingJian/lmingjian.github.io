---
title: Windows 系统的端口占用检查
date: 2024-01-03
author: LMingJian
---

## Question

在启动某些服务，如 Nginx 时，有可能会出现报错 `bind() to 0.0.0.0:80 failed` 提示端口绑定失败，端口被占用。

## Step

1. 按下 `Win+R`，然后输入 `cmd`，打开终端。
2. 执行 `netstat -aon | findstr :80`，查找端口 80 的使用进程，记录进程的 `pid`。
3. 执行 `tasklist | findstr PID`，找到对应 `pid` 进程的服务，然后手动关闭该服务。
4. 步骤 3 也可以在任务管理器的详细信息栏中进行。



