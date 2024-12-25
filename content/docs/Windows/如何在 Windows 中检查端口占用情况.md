---
title: 如何在 Windows 中检查端口占用情况
date: 2024-12-25T16:17:24+08:00
author: LiangMingJian
---

# 问题

当用户在 Windows 启动某些服务，如 Nginx 时，操作系统有可能会出现报错 `bind() to 0.0.0.0:80 failed` 提示端口绑定失败，端口被占用。

# 解决

1.按下 `Win+R`，然后输入 `cmd`，打开终端。

2.执行 `netstat -aon | findstr :80`，查找端口 80 的使用进程，记录进程的 `pid`。

3.执行 `tasklist | findstr PID`，找到对应 `pid` 进程的服务，然后手动关闭该服务。

4.步骤 3 也可以在任务管理器的详细信息栏中进行。
