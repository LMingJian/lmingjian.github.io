---
title: PyQt5 QDialog 消息提示类
date: 2020-12-19
author: LM
---

## 1.QDialog 的子类

QDialog 是 PyQt 中所有消息窗口的父类，其中包括多种窗口子类，如 QMessageBox，QFileDialog，QColorDialog，QFontDialog，QInputDialog 等。

## 2.QDialog 类中的常用方法

```python
# 方法
setWindowTitle()  # 设置对话框标题
setWindowModality()  # 设置窗口模态
# 可用属性
Qt.NonModal: 非模态,可以和程序的其他窗口进行交互
Qt.WindowModal: 窗口模态,程序在未处理玩当前对话框时,将阻止和对话框的父窗口进行交互
Qt.ApplicationModal: 应用程序模态,阻止和任何其他窗口进行交互
```

