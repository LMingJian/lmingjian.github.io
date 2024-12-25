---
title: 如何在 Linux 中检查命令是否存在
date: 2024-12-25T13:55:53+08:00
author: LiangMingJian
---

# 实现

在 Linux 中，你可以使用`command -v`来检查特定命令是否存在，在 Bash 脚本中使用以下代码检查。

```bash
if ! command -v <the_command> &> /dev/null
then
    echo "<the_command> could not be found"
    exit
fi
```

在一些特别的环境中，还可以使用`hash`，`type`来进行检查。

```bash
hash <the_command> # For regular commands. Or...
type <the_command> # To check built-ins and keywords
```

另外，请避免使用`which`。在许多操作系统中`which`不会设置退出状态，其不会返回值。意味着如果`if which foo`不会返回`foo`不存在，总会报告`foo`存在。此外，`which`还会将输出更改或将结果挂载在包管理器中。因此请尽量避免使用`which`，请改用以下方法将结果输出到空`2>/dev/null`，以避免程序出错。

```bash
$ command -v foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ type foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ hash foo 2>/dev/null || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
```

一个简单的函数示例如下，如果命令存在则运行，否则返回 `gdate date`

```bash
gnudate() {
    if hash gdate 2>/dev/null; then
        gdate "$@"
    else
        date "$@"
    fi
}
```

{{< details "参考文件" >}} 
1：[ check if a program exists from a Bash script? @Stack Overflow ](https://stackoverflow.com/questions/592620/how-can-i-check-if-a-program-exists-from-a-bash-script)
{{< /details >}}
