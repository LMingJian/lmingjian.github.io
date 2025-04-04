---
title: Python 的优缺点
date: 2024-12-25T16:11:12+08:00
author: LiangMingJian
---

# Python 的优点

① Python 的定位是优雅、明确、简单，所以 Python 程序看上去总是简单易懂，初学者学 Python，不但入门容易，而且将来深入下去，可以编写那些非常非常复杂的程序。

② Python 的开发效率非常高，Python 有非常强大的第三方库，基本上你想通过计算机实现任何功能，Python 官方库里都有相应的模块进行支持，直接下载调用后，在基础库的基础上再进行开发，大大降低开发周期，避免重复造轮子。

③ Python 是高级语言。当你用 Python 语言编写程序的时候，你无需考虑诸如如何管理你的程序使用的内存一类的底层细节。

④ Python 具有很高的可移植性。由于它的开源本质，Python 已经被移植在许多平台上（经过改动使它能够工作在不同平台上）。如果你小心地避免使用依赖于系统的特性，那么你的所有 Python 程序无需修改就几乎可以在市场上所有的系统平台上运行。

⑤ Python 的可扩展性。你可以把你的部分程序用 C 或 C++ 编写，然后在你的 Python 程序中使用它们。

⑥ Python 的可嵌入性。你可以把 Python 嵌入你的 C/C++ 程序，从而向你的程序用户提供脚本功能。

# Python 的缺点

① Python 的运行速度相比 C 语言会慢很多，跟 Java 相比也要慢一些，但其实在大多数情况下 Python 已经完全可以满足你对程序速度的要求，除非你要写对速度要求极高的搜索引擎等。

② Python 的代码不能加密，因为 Python 是解释性语言，所以源码都是以明文形式存放的。

③ Python 的线程不能利用多 CPU 运行，Python 的线程是操作系统的原生线程，因此 Python 的运行受到 GIL 的影响，GIL 即全局解释器锁（Global Interpreter Lock），是计算机程序设计语言解释器用于同步线程的工具，其功能是使得任何时刻仅有一个线程在执行。一个 Python 解释器进程内有一条主线程，以及多条用户程序的执行线程。即使在多核 CPU 平台上，由于 GIL 的存在，Python 也会禁止多线程的并行执行。
