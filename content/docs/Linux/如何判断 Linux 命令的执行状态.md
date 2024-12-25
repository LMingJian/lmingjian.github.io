---
title: 如何判断 Linux 命令的执行状态
date: 2024-12-25T13:55:17+08:00
author: LiangMingJian
---

# 需求

在编写 shell 脚本时，有时候需要检查前一命令是否执行成功，此时可以在 if 条件中使用 $? 来检查前一命令的结束状态。

# 示例

```
root@localhost:~# ls /usr/bin/
root@localhost:~# echo $?
```

- 如果结束状态是 0，说明前一个命令执行成功。
- 如果结束状态不是 0，说明命令执行失败。
