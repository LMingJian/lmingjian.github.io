---
title: 什么是 Shell？什么是 Bash？
date: 2024-12-25T10:33:03+08:00
author: LiangMingJian
---

## 什么是 Shell

在计算机科学中，**Shell 俗称壳（用来区别于核），是指为使用者提供操作界面的软件（命令解析器）**。它接收用户命令，然后调用相应的应用程序。

> 核指的是计算机操作系统内核（Kernel），内核是管理 CPU、内存等硬件的核心软件。

**Shell 是一个命令语言的同时，它也是一种程序设计语言。**

**作为命令语言时**，它可以使用一个操作界面，**交互式**地解释和执行用户输入的命令。

> 交互式是指用户输入一个指令时，Shell 能马上提供反馈结果，Shell 可以与用户进行实时交互。

**作为程序设计语言时**，它支持定义各种变量和参数，并提供了许多在高级语言中才具有的控制结构，包括循环和分支，并支持函数编程，给用户提供脚本编写的功能。

Shell 是操作系统最外面的一层，是文字操作系统与外部最主要的接口，管理用户与操作系统之间的交互。

## Shell 的两大类

### 图形 Shell（Graphical User Interface Shell，GUI Shell）

我们常用的 Windows Explorer 文件管理系统就是这样一个 GUI Shell，通过可视化操作，我们可以管理系统中的文件。当然 Linux 中的 GNOME 也是这样的一个 GUI Shell。

![](_images/drawingbed/img/Pasted%20image%2020251216164405.png)

### 命令行 Shell（Command Line Interface Shell ，即CLI Shell）

这个就包括 Linux 系统中的 bash / sh / ksh / csh / zsh，Windows 中的 cmd.exe / PowerShell，这些都属于 CLI Shell。

![](_images/drawingbed/img/Pasted%20image%2020251216164410.png)

## 什么是 Bash

Bash（GNU Bourne-Again Shell）是一个为 GNU 计划编写的 Unix Shell 的一种，简单来说，它就是一个**命令处理器**，用来执行用户输入的各种指令。

现代大多数的 Linux 系统的 Shell 命令控制终端都缺省（默认）为 Bash，为用户提供包括交互式命令执行，Shell 脚本编写等功能。

> GNU 计划是由理查德·斯托曼（Richard Stallman）发起的自由软件协作项目，旨在创建一套**完全自由的操作系统**。该计划的核心是 GNU 通用公共许可证（GPL），允许用户自由使用、修改和分发软件。  

> UNIX 系统是贝尔实验室开发的操作系统，其名称源于对 Multics 项目（多路信息与计算系统）的双关语 Unics（单路信息与计算系统），该操作系统是现代所有 Linux 的基础。
