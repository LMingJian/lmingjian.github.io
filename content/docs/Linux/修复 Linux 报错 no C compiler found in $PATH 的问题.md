---
title: 修复 Linux 报错 no C compiler found in $PATH 的问题
date: 2024-12-25T13:56:12+08:00
author: LiangMingJian
---

# BUG

在 Linux 编译某些程序时，出现报错：

```bash
configure: error: no acceptable C compiler found in $PATH
```

# Resolution

Linux 缺少合适的 C 编译器。

```bash
yum install gcc-c++
```

# 拓展阅读

**gcc 是什么？**

gcc 是 GNU Compiler Collection 的缩写，是 GNU 开发的 C 编译器。在 gcc 的逐渐更新中，其逐渐接受 C++，Objective-C、Java、Go 等其他语言的编译工作。

**gcc-c++ 是什么？**

gcc 通常在编译 C++ 源代码的阶段，只能编译 C++ 源文件，而不能自动和 C++ 程序使用的库链接。因此，通常建议安装 gcc-c++ 来使用 g++ 命令来完成 C++ 程序的编译和连接， g++  会自动调用 gcc 实现编译。

>编译过程分为编译、链接两个阶段，首先源程序文件会被编译成目标文件，然后多个目标文件会连同库被链接成一个可执行文件，最终，这个可执行文件可以被加载到内存中运行。
