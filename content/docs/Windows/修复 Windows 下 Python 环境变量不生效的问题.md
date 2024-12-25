---
title: 修复 Windows 下 Python 环境变量不生效的问题
date: 2024-12-25T16:17:37+08:00
author: LiangMingJian
---

# BUG 描述

已在 Window10 上下载配置好 Python，但是在命令行 CMD 中使用 Python 命令时提示`Python not found; run without arguments to install from the Microsoft Store`，已确认 Python 的环境变量已配置。

# Resolution

环境变量的优先级问题，由于 WindowsApp 的环境路径优先于 Python 的路径，因此当调用 Python 时，会优先询问 WindowsApp。调换两者路径即可解决。

![](/_images/drawingbed/img/202205051003225.png)

进行调换后。

![](/_images/drawingbed/img/202205051003370.png)

{{< details "参考文件" >}} 
1：[ 命令窗口不能使用Python ](https://zhuanlan.zhihu.com/p/380716375)
{{< /details >}}
