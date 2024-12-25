---
title: 如何重定向 Linux 的输出到空文件
date: 2024-12-25T13:56:02+08:00
author: LiangMingJian
---

# 空文件 /dev/null 

代表空设备文件，垃圾箱一类的文件，类似于文件 **/dev/zero**（这个文件只会输出0）

# 重定向命令 1>/dev/null 与 2>&1 的含义

在 Shell 中，每个进程都和三个系统文件相关联：标准输入 stdin，标准输出 stdout、标准错误 stderr，三个系统文件的文件描述符分别为 0，1，2

- **1>/dev/null** ： 表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
- **2>&1** ：表示标准错误输出重定向到（等同于）标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。
- **>** ：代表重定向到哪里，例如：echo "123" > /home/123.txt
- **1** ：表示 stdout 标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
- **2** ：表示 stderr 标准错误
- **&** ：表示等同于的意思，2>&1，表示 2 的输出重定向等同于 1

# 示例

```bash
$ cat test.sh
# test.sh 中包含两个命令（t, date），其中 t 是一个不存在的命令，执行会报错，默认情况下，错误会输出到 stderr。date 则能正确执行，并且输出时间信息，默认输出到 stdout

./test.sh > test1.log
# ./test.sh: line 1: t: command not found
$ cat test1.log
# Wed Jul 10 21:12:02 CST 2013
# 可以看到，date 的执行结果被重定向到 log 文件中了，而 t 无法执行的错误则只打印在屏幕上。

$ ./test.sh > test2.log 2>&1
$ cat test2.log
# ./test.sh: line 1: t: command not found
# Tue Oct 9 20:53:44 CST 2007
# 这次，stderr 和 stdout 的内容都被重定向到 log 文件中了。
# 如果只想重定向标准错误到文件中，则可以使用 2>file。
```

- **1>test.log 2>&1** ：stdout 直接送往文件 test.log ，stderr 是继承了 stdout 的管道之后，再被送往文件 test.log 。test.log 文件只被打开一遍，只使用了一个管道 stdout，但已经包括了 stdout 和 stderr。
- 注意的是：可以写成**1>test.log 2>test.log** ，但此时， stdout 和 stderr 会竞争打开test.log 文件，test.log 会被打开两遍，由此导致 stdout 和 stderr 互相覆盖。

# Ex.为何 2>&1 要写在后面呢？

- **command>file 2>&1** ：首先是 command>file 将标准输出重定向到 file 中， 2>&1 标准错误拷贝了标准输出的行为，也就是同样被重定向到 file 中，最终结果就是标准输出和错误都被重定向到 file 中。 
- **command 2>&1 >file**：2>&1 标准错误拷贝了标准输出的行为，但此时标准输出还是在终端，>file 后输出才被重定向到file，所以标准错误还是会打印在终端中。
