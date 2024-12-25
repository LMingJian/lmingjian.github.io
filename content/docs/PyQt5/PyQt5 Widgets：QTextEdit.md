---
title: PyQt5 Widgets：QTextEdit
date: 2024-12-25T16:07:22+08:00
author: LiangMingJian
---

# 概述

QTextEdit 是 Qt 提供的文本组件，与 QLineEdit 不同的是，**QTextEdit 支持多行文字，以及 HTML 的输入。**

# 支持的方法

```python
setPlaceholderText()  #| 设置占位文本
placeholderText()     #| 获取占位文本
setPlainText(str)     #| 设置普通文本
insertPlainText(str)  #| 插入普通文本
toPlainText()         #| -> str 返回普通文本
setHtml(str)          #| 设置Html文本
insertHtml(str)       #| 插入Html文本
toHtml()              #| -> str 返回Html 文本
setText(str)          #| 设置文本（自动识别）
append(str)           #| 追加文本
clear()               #| 清空文本
setFontPointSize(float)  #| 设置字体大小
setFontFamily(str)       #| 设置字体
```
