---
title: 如何在 Linux 后台运行程序
date: 2024-12-25T13:55:32+08:00
author: LiangMingJian
---

# 实现后台执行程序

在 Linux 中，有时候需要执行一些要在后台长期挂载的命令或任务。此时，用户可以通过使用 `nohup` 和 `&`来使命令在后台执行，不受终端关闭或 Ctrl+C 影响。

```python
# 后台执行程序
nohup python test.py > run.log 2>&1 &
# 查看后台程序，aux 中 a:显示所有程序；u:以用户为主的格式来显示；x:显示所有程序，不以终端机来区分
ps aux |grep "test.sh"  
# -er 中 -e:显示所有进程;-f:全格式
ps -ef |grep "test.sh"  
# 关闭后台程序，-9:表示强制关闭
kill -9 1001
# 根据名称删除
pkill -f Chrome
# kill 对应的是 PID
# pkill 对应的是 COMMAND
```

# Ex.命令 nohup 和 & 的区别

- **&**：表示程序在后台运行，当执行`./a.out &`的时候，即使你使用 Ctrl+C，a.out 照样运行（因为对 SIGINT 信号免疫）。但是要注意，如果你直接关掉 Bash ，那么，a.out 进程会停止关闭。可见，& 的后台并不硬（因为对 SIGHUP 信号不免疫）。
- **nohup**：表示的是忽略 SIGHUP 信号，所以当运行`nohup ./a.out`时，关闭 Bash，a.out 还是在运行（对 SIGHUP 信号免疫）。但是，如果直接在 Bash 中使用 Ctrl+C，那么，a.out 进程会停止关闭（因为对 SIGINT 信号不免疫）。

所以， & 和 nohup 两者并没有直接关系， 要让进程真正不受 Bash 中 Ctrl+C 和 shell 关闭的影响，最好是使用命令`nohua ./a.out &` 。
