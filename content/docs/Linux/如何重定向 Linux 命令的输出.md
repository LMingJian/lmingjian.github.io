---
title: 如何重定向 Linux 命令的输出
date: 2024-12-25T13:56:07+08:00
author: LiangMingJian
---

# 需求

有时候用户需要将 Linux 命令的执行结果保存到文件中，此时就需要使用重定向功能，将命令输出到某个指定文件中。

# 实现

Linux 中使用 > 和 >> 来重定向标准输出。其中 > 在输出时会强制覆盖原内容，而 >> 会使用追加模式。

```
ping baidu.com > A.txt
ping baidu.com >> A.txt
```
