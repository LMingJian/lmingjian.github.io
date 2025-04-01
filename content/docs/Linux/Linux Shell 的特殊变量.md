---
title: Linux Shell 的特殊变量
date: 2025-04-01T15:23:44+08:00
author: LiangMingJian
---

# $n

含义：用于接收 shell 脚本文件执行时传入的参数，`$0`用于获取当前脚本文件名称，`$1~$9`代表输入的第 1 到第 9 个参数。需要注意的是，第 10 个以上的参数需要使用`${10}, ${11}`，使用括号包裹数字。

```shell
#!/bin/bash

echo $0
echo "输入的第1个参数：$1"
echo "输入的第2个参数：$2"
echo "输入的第11个参数：${11}"
```

执行上述脚本：`bash test.sh 1 2 3 4 5 6 7 8 9 10 11`，获取的结果如下：

```
test.sh
输入的第1个参数：1
输入的第2个参数：2
输入的第11个参数：11
```

# $\#

含义：获取 shell 脚本所有输入参数的个数。

```shell
echo "参数个数：${#}"
echo 参数个数：$#
# 上面两个语句等效，输出结果一致。
```

# $* 和 $@

含义：获取所有传入参数，使用`"$*"`输出参数，结果是拼接起来的字符串，使用`"$@"`输出参数，结果是数组。

```shell
#!/bin/bash

for item in "$*"   # 结果就是全部打印出来
do
    echo $item
done             
          
echo "************************"
             
for item in "$@"
do
    echo $item     # 这里也可以${item}
done 
```

执行上述脚本：`bash test.sh 1 nihao 12 hello abc 123`，获取的结果如下：

```
1 nihao 12 hello abc 123
************************
1
nihao
12
hello
abc
123
```

# $?

含义：用于获取上一个 shell 命令的退出状态码，或者是函数的返回值。

```shell
echo "hello" 
echo $?  # 上一条执行成功，这里返回 0
```

# $\$

含义：用于获取当前 shell 环境的进程 ID 号。

```shell
ps aux | grep bash
echo $$  # 在交互式 shell 下，上述结果是一样的
```
