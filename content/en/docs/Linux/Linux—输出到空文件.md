---
title: Linux 输出到空文件
date: 2020-11-16
author: LM
---

## 1.文件 /dev/null 

代表空设备文件，垃圾箱一类的文件，类似于文件 **/dev/zero**（这个文件只会输出0）

## 2.语句 1 > /dev/null 2>&1 的含义

在 Bash 中，每个进程都和三个系统文件相关联：标准输入stdin，标准输出stdout、标准错误stderr，三个系统文件的文件描述符分别为0，1，2

- **1 > /dev/null** ： 表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
- **2>&1** ：表示标准错误输出重定向到（等同于）标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。
- **>** ：代表重定向到哪里，例如：echo "123" > /home/123.txt
- **1** ：表示 stdout 标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
- **2** ：表示 stderr 标准错误
- **&** ：表示等同于的意思，2>&1，表示2的输出重定向等同于1

## 3.实例分析

- **1 >a 2>a** ：stdout和stderr都直接送往文件 a ，a文件会被打开两遍，由此导致stdout和stderr互相覆盖，两者互相竞争使用文件 a 的管道
- **1 >a 2>&1** ：stdout直接送往文件a ，stderr是继承了stdout的管道之后，再被送往文件a 。a文件只被打开一遍，就是stdout将其打开。只使用了一个管道stdout，但已经包括了stdout和stderr。从IO效率上来讲，1 >a 2>&1的效率更高。

```bash
$ cat test.sh
#test.sh中包含两个命令（t, date），其中t是一个不存在的命令，执行会报错，默认情况下，错误会输出到stderr。date则能正确执行，并且输出时间信息，默认输出到stdout

./test.sh > test1.log
#./test.sh: line 1: t: command not found
$ cat test1.log
#Wed Jul 10 21:12:02 CST 2013
#可以看到，date的执行结果被重定向到log文件中了，而t无法执行的错误则只打印在屏幕上。

$ ./test.sh > test2.log 2>&1
$ cat test2.log
#./test.sh: line 1: t: command not found
#Tue Oct 9 20:53:44 CST 2007
#这次，stderr和stdout的内容都被重定向到log文件中了。
#如果只想重定向标准错误到文件中，则可以使用2>file。
```

其他例子：

- ls 2>1，不会报没有2文件的错误，但会输出一个空的文件1
- ls xxx 2>1测试，没有xxx这个文件的错误输出到了1中
- ls xxx 2>&1测试，不会生成1这个文件了，不过错误跑到标准输出了
- ls xxx >out.txt 2>&1，实际上可换成 ls xxx 1>out.txt 2>&1，错误和输出都传到out.txt

## 4.为何 2>&1 要写在后面？

- **command > file 2>&1** ：首先是command > file将标准输出重定向到file中， 2>&1 是标准错误拷贝了标准输出的行为，也就是同样被重定向到file中，最终结果就是标准输出和错误都被重定向到file中。 
- **command 2>&1 >file**：2>&1 标准错误拷贝了标准输出的行为，但此时标准输出还是在终端。>file 后输出才被重定向到file，但标准错误仍然保持在终端。