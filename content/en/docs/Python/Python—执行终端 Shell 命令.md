---
title: Python 执行 Shell 命令
date: 2021-01-14
author: LM
---

## 1.需求

通过 Python 执行系统终端命令。

## 2.实现：使用 subprocess 模块的 call 方法

使用 subprocess 模块中的 call 函数传递命令给 Linux Shell 系统

```python
>>> from subprocess import call  
>>> call(["ls", "-l"])
```

## 3.实现：使用 os 模块的 system 方法

system 方法会创建子进程来运行外部程序，因此只会返回外部程序的运行结果，这个方法适用于外部程序没有输出结果的情况

```bash
>>> import os  
>>> os.system("echo \"Hello World\"")   # 直接使用 os.system 调用一个 echo 命令  
Hello World # 打印结果  
>>> val = os.system("ls -al | grep \"log\" ")   # 使用 val 接收返回值  
-rw-r--r--  1 root  root  6030829 Dec 31 15:14 log  # 此时只打印了命令结果  
>>> print(val)
0   # 注意，命令正常运行，返回值是 0
```

## 4.实现：使用 os 模块的 popen 方法

popen 方法会创建管道来联通外部程序，当需要得到外部程序的输出结果时，可以调用 read 方法来读取输出内容。

```bash
>>> os.popen('ls -lt')                  # 调用 os.popen(cmd) 并不能得到我们想要的结果  
<open file 'ls -lt ', mode 'r' at 0xb7585ee8>  
>>> print(os.popen('ls -lt').read())    # 调用 read() 方法可以得到命令的结果  
total 6064  
-rwxr-xr-x 1 long       long            23 Jan  5 21:00 hello.sh  
-rw-r--r-- 1 long       long           147 Jan  5 20:26 Makefile  
drwxr-xr-x 3 long       long          4096 Jan  2 19:37 test  
-rw-r--r-- 1 root       root       6030829 Dec 31 15:14 log  
drwxr-xr-x 2 long       long          4096 Dec 28 09:36 pip_build_long  
drwx------ 2 Debian-gdm Debian-gdm    4096 Dec 23 19:08 pulse-gylJ5EL24GU9  
drwx------ 2 long       long          4096 Jan  1  1970 orbit-long  
>>> val = os.popen('ls -lt').read()     # 使用变量接收命令返回值
```

