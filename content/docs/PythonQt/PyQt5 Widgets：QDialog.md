---
title: PyQt5 Widgets：QDialog
date: 2024-12-25T16:06:48+08:00
author: LiangMingJian
---

# 概述

QDialog 是 Qt 中所有消息窗口的父类，其中包括多种窗口子类，如 QMessageBox，QFileDialog，QColorDialog，QFontDialog，QInputDialog 等。

# PyQt5 中支持的方法

```python
# 方法
setWindowTitle()  # 设置对话框标题
setWindowModality()  # 设置窗口模态
# 可用属性
Qt.NonModal: 非模态,可以和程序的其他窗口进行交互
Qt.WindowModal: 窗口模态,程序在未处理玩当前对话框时,将阻止和对话框的父窗口进行交互
Qt.ApplicationModal: 应用程序模态,阻止和任何其他窗口进行交互
```
