---
title: PyQt5 QTextEdit 文本元件
date: 2020-12-23
author: LM
---

## 1.QTextEdit 文本组件

QTextEdit 是 PyQt 提供的文本组件，与 QLineEdit 不同的是，QTextEdit 支持多行文字，以及 HTML 的输入。

## 2. 设置文本格式

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
```


## 3.设置字体和大小

```python
setFontPointSize(float)  # 设置字体大小
setFontFamily(str)       # 设置字体
```
