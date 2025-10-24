---
title: 如何在 Linux 中检查命令是否存在
date: 2024-12-25T13:55:53+08:00
author: LiangMingJian
---

# 需求

在编写 Linux 运维脚本时，有时候会碰见这样的情况——需要使用一些特殊的命令来执行操作，这些特殊的命令有概率不被安装在目标系统上，需要用户手动安装。

此时，如果在 Shell 脚本中检查该特殊命令是否存在就成了一个问题。

# 通过 command 命令来实现

在 Linux 中，command 是一个用于确定给定命令类型和位置的命令，其使用手册如下：

```shell
command [-pVv] target_command [arg ...]
# -p: 使用一个安全的路径来执行命令，忽略用户定义的路径变量（command -p ls -l）
# -V：打印详细的命令描述，V 是 Verbose 详细的意思（command -V ls）
# -v：同样是打印命令描述，但展示较少数据（command -v ls）
```

显然，通过检查 `command -v target_command` 的退出状态码便可以实现检查命令是否存在的需求（**当命令不存在时，command 的状态码会返回非 0，反之返回 0**）。

在 Shell 脚本中，可以使用以下代码：

```bash
if ! command -v unzip &> /dev/null
then
    echo "unzip could not be found"
    exit
fi
# command -v unzip 返回命令的描述，用来判断命令是否存在
# &> /dev/null 将标准输出和标准错误都重定向到空设备，不显示任何输出
# ! 将结果取反，用来打印结果
```

另外，还可以尝试使用 `hash`，`type` 来实现检查操作，这些命令也是有效的。

```bash
hash unzip
# hash 是用于管理命令路径缓存一个工具
# 通过 hash target_command，可以将已有命令写入缓存
# 但如果是没有的命令，则会报错 -bash: hash: unzip: not found

type unzip
# type 是用来显示指定命令类型的工具
# 通过 type target_command，可以显示已有命令的类型
# 但如果是没有的命令，则会报错 -bash: type: unzip: not found
```

最后，请尽量避免使用 `which` 命令（用于返回目标命令在环境变量中的绝对路径）。

虽然通过 `which` 命令也能检查到命令是否存在。但是在一些操作系统中，**`which` 命令不会设置退出状态，因此其不会有返回值**。

这就意味着，如果用户在 Shell 脚本中编写 `if which unzip`，那么脚本将不会返回命令不存在，而是会报告命令存在。

——————————

> [ check if a program exists from a Bash script? @Stack Overflow ](https://stackoverflow.com/questions/592620/how-can-i-check-if-a-program-exists-from-a-bash-script)
