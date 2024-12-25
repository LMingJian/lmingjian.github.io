---
title: Linux Command：find
date: 2024-12-25T13:57:45+08:00
author: LiangMingJian
---

# 概述

find 命令用于在指定目录下查找文件和目录，它可以使用不同的选项来过滤和限制查找的结果。

```shell
find [路径] [匹配条件] [动作]
```

# 支持的参数

```shell
-name pattern # 按文件名查找，支持使用通配符 * 和 ?。
-type type # 按文件类型查找，可以是 f（普通文件）、d（目录）、l（符号链接）等。
-size [+-]size[cwbkMG] # 按文件大小查找，支持使用 + 或 - 表示大于或小于指定大小，单位可以是 c（字节）、w（字数）、b（块数）、k（KB）、M（MB）或 G（GB）。
-mtime days # 按修改时间查找，支持使用 + 或 - 表示在指定天数前或后，days 是一个整数表示天数。
-user username # 按文件所有者查找。
-group groupname # 按文件所属组查找。
-amin n # 查找在 n 分钟内被访问过的文件。
-atime n # 查找在 n*24 小时内被访问过的文件。
-cmin n # 查找在 n 分钟内状态发生变化的文件（例如权限）。
-ctime n # 查找在 n*24 小时内状态发生变化的文件（例如权限）。
-mmin n # 查找在 n 分钟内被修改过的文件。
-mtime n # 查找在 n*24 小时内被修改过的文件。
```

# 按照文件名查找

```shell
find / -name httpd.conf　　# 在根目录下查找文件 httpd.conf
find /etc -name httpd.conf　　# 在 /etc 目录下文件 httpd.conf
find /etc -name '*srm*'　　# 使用通配符 *，在 /etc 目录下查找文件名中含有字符串 srm 的文件
```

# 按照文件大小查找 　　　　

```shell
find / -empty 　　   # 查找在系统中为空的文件或者文件夹
find / -size +1G　　# 查找出大于 1G 的文件
find / -size -1000k 　　# 查找出小于 1000KB 的文件
find /tmp -size +10000c -and -mtime +2  # 在 /tmp 目录下查找大于 10000 字节并在最后 2 分钟内修改的文件
find / -size +1G -or -user george 　　    # 在 / 目录下查找大于 1G 的文件或用户是 george 的文件
find /tmp ! -size +1G　　                # 在 /tmp 目录中查找不大于 1G 的文件
```
