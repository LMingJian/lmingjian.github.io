---
title: Linux Command：tail
date: 2024-12-25T13:58:29+08:00
author: LiangMingJian
---

# 概述

tail 命令可用于查看文件的内容（默认查看最新的 10 行内容），携带参数 -f 时，常用于查阅正在改变的日志文件。

tail -f filename 会把 filename 文件里的最尾部的内容显示在屏幕上，并且不断刷新，只要 filename 更新就可以看到最新的文件内容。

```
tail [参数] [文件]
```

# 支持的参数

```
-f          # 循环读取
-q          # 不显示处理信息
-v          # 显示详细的处理信息
-c Number   # 显示的字节数 Number 的内容
-n L        # 显示文件的尾部 L 行内容
--pid=PID   # 与 -f 合用，表示在进程 PID 死掉之后结束
-q, --quiet, --silent   # 从不输出给出文件名的头部
-s, --sleep-interval=S  # 与 -f 合用，表示在每次反复的间隔休眠 S 秒
```
