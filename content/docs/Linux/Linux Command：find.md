---
title: Linux Command：find
date: 2024-12-25T13:57:45+08:00
author: LiangMingJian
---

# 概述

find 命令用于在指定目录下查找文件和目录，它可以使用不同的选项来过滤和限制查找的结果。

```shell
find [路径] [匹配条件] [匹配内容]
```

# 按文件名查找

**参数**：`-name pattern`

支持使用通配符来模糊匹配文件和目录，支持的通配符有：

- `*`：匹配零个或任意多个字符
- `?`：匹配单个任意字符
- `[]`：匹配括号内限定的字符

```shell
# 在根目录下查找文件 httpd.conf
find / -name httpd.conf

# 在 /etc 目录下文件 httpd.conf
find /etc -name httpd.conf

# 使用通配符 *，在 /etc 目录下查找文件名中含有字符串 srm 的文件
find /etc -name '*srm*'
```

# 按文件大小查找

**参数**：`-size [+-]size[cwbkMG]`

支持使用参数 + 或 - 来匹配大于或小于指定大小的文件，支持的单位可以是 c（字节）、w（字数）、b（块数）、k（KB）、M（MB）或 G（GB）。

特别的，提供 `-empty` 参数用于查找空文件。

```shell
# 查找在系统中为空的文件或者文件夹
find / -empty 　　   

# 查找出大于 500MB 的文件
find / -size +500M　

# 查找出大于 1G 的文件
find / -size +1G　

# 查找出小于 1G 的文件
find / -size +1G

# 在 /tmp 目录中查找不大于 1G 的文件
find /tmp ! -size +1G　                
```

# 其他的搜索条件

除了上述按文件名和文件大小的查找外，find 命令还支持以下的查找功能。

**参数**：`-type type`，按文件类型查找，可以是 f（普通文件）、d（目录）等。

```shell
find /root -type d
```

**参数**：`-mtime days`，按修改时间查找，支持使用 + 或 - 表示在指定天数前或后，days 是一个整数表示天数。

```shell
# 查找修改时间在 1 天内的文件或目录
find /root -mtime -1

# 查找修改时间在 1 个月前的文件或目录
find /root -mtime +30
```

**参数**：`-user username` 和 `-group groupname`，按文件所有者或所有组进行查找。

```shell
# 查找用户为 root 创建的文件
find /root -user root

# 查找用户组为 root 的文件
find /root -group root
```

**其他参数**

```shell
-amin n    # 查找在 n 分钟内被访问过的文件。
-atime n   # 查找在 n*24 小时内被访问过的文件。

-cmin n    # 查找在 n 分钟内状态发生变化的文件（例如权限）。
-ctime n   # 查找在 n*24 小时内状态发生变化的文件（例如权限）。

-mmin n    # 查找在 n 分钟内被修改过的文件。
-mtime n   # 查找在 n*24 小时内被修改过的文件。
```

# 搜索条件的组合使用

在 find 命令中，支持通过逻辑运算符 `-a 或 -and`，`-o 或 -or`，`! 或 -not` 来组合使用多个搜索条件。

特别的，一般情况下 `-a 或 -and` 可以省略，因为它是默认的逻辑操作符，在多个参数传入时自动使用。

```shell
# 查找 /home 目录下以 .txt ‌且小于 100KB 的文件
# 默认省略了逻辑运算符，当然也可以加上逻辑运算符
find /home -name "*.txt" -size -100k
find /home -name "*.txt" -and -size -100k

# 查找 /home 目录下以 .txt ‌或 .sh 结尾的文件
find /home -name "*.txt" -or -name "*.sh"

# 查找 /home 目录下不以 .sh 结尾的文件
find /home -not -name "*.sh"
```

另外，当逻辑条件比较复杂时，通常我们可以使用小括号 `()` 来明确优先级。但由于小括号在 Linux 终端中有其他用途，因此需要使用 `\( 和 \)` 或者 `'(' 和 ')'` 来代替。

```shell
# 查找 /home 目录下以 .txt 或 .sh 结尾，并且文件大小小于 100KB 的文件
find /home \( -name "*.txt" -o -name "*.sh" \) -a -size -100k
find /home '(' -name "*.txt" -o -name "*.sh" ')' -a -size -100k
```

——————————

> [ find man docs ](https://www.man7.org/linux/man-pages/man1/find.1.html)
