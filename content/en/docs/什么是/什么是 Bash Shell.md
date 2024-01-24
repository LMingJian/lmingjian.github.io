---
title: 什么是 Bash Shell
date: 2020-12-09
author: LM
---

## 1.什么是 Bash

Bash 是 Unix shell 的一种，是一个命令处理器，运行于大多数类 Unix 系统的操作系统之上，Linux 与 Mac OS 都将它作为默认 shell。

## 2.什么是 Shell

在计算机科学中，**Shell 俗称壳（用来区别于核），是指为使用者提供操作界面的软件（命令解析器）**。它接收用户命令，然后调用相应的应用程序。

同时它又是一种程序设计语言。作为命令语言，它交互式解释和执行用户输入的命令或者自动地解释和执行预先设定好的一连串的命令。同时，它定义了各种变量和参数，并提供了许多在高级语言中才具有的控制结构，包括循环和分支。

Shell 是操作系统最外面的一层，是文字操作系统与外部最主要的接口，管理用户与操作系统之间的交互。

## 3.Shell 的两大类

### a.图形界面 shell（Graphical User Interface shell 即 GUI shell）

例如：应用最为广泛的 Windows Explorer （微软的windows系列操作系统），还有也包括广为人知的 Linux shell，其中linux shell 包括 X window manager (BlackBox和FluxBox），以及功能更强大的CDE、GNOME、KDE、 XFCE。

### b.命令行式 shell（Command Line Interface shell ，即CLI shell）

例如：bash / sh / ksh / csh / zsh（Unix/linux 系统）（MS-DOS系统），cmd.exe / 命令提示字符（Windows NT 系统），Windows PowerShell（支持 .NET Framework 技术的 Windows NT 系统）

## 4.交互式 shell 和非交互式 Shell

- **交互式 Shell**：就是 shell 等待你的输入，并且执行你提交的命令。这种模式被称作交互式是因为 shell 与用户进行交互。这种模式也是大多数用户非常熟悉的：登录、执行一些命令、签退。当你签退后，shell 也终止了。
- **非交互式 Shell**：在这种模式下，shell 不与你进行交互，而是读取存放在文件中的命令，并且执行它们。当它读到文件的结尾，shell 也就终止了。

## 5.Linux 的 Shell

Linux 操作系统**缺省（默认）的 Shell 是 Bourne Again shell**，它是 Bourne shell 的扩展，简称 **Bash**，与 Bourne shell 完全向后兼容，并且在 Bourne shell 的基础上增加、增强了很多特性。Bash 放在 /bin/bash 中，它有许多特色，可以提供如命令补全、命令编辑和命令历史表等功能，它还包含了很多 C shell 和 Korn shell 中的优点，有灵活和强大的编程接口，同时又有很友好的用户界面。
