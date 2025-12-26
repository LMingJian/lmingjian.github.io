---
title: PyQt5 Widgets：QLineEdit
date: 2024-12-25T16:07:05+08:00
author: LiangMingJian
---

# 概述

QLineEdit 是 Qt 提供的一个文本组件，用户可以通过该组件输入文本信息。

# 支持的方法

```python
setText(str)  # 设置普通文本
Text()        # 返回普通文本
```

# 使用掩码限制输入格式

```python
ipLineEdit.setInputMask('000.000.000.000;_')
macLineEdit.setInputMask('HH:HH:HH:HH:HH:HH;_')
dateLineEdit.setInputMask('0000-00-00')
licenseLineEdit.setInputMask('>AAAAA-AAAAA-AAAAA-AAAAA-AAAAA;#')
```

# Ex.支持的掩码类型

```
# 掩码类型
A    ASCII字母字符是必须输入的(A-Z、a-z)
a    ASCII字母字符是允许输入的,但不是必需的(A-Z、a-z)
N    ASCII字母字符是必须输入的(A-Z、a-z、0-9)
n    ASII字母字符是允许输入的,但不是必需的(A-Z、a-z、0-9)
X    任何字符都是必须输入的
x    任何字符都是允许输入的,但不是必需的
9    ASCII数字字符是必须输入的(0-9)
0    ASCII数字字符是允许输入的,但不是必需的(0-9)
D    ASCII数字字符是必须输入的(1-9)
d    ASCII数字字符是允许输入的,但不是必需的(1-9)
#    ASCI数字字符或加减符号是允许输入的,但不是必需的
H    十六进制格式字符是必须输入的(A-F、a-f、0-9)
h    十六进制格式字符是允许输入的,但不是必需的(A-F、a-f、0-9)
B    二进制格式字符是必须输入的(0,1)
b    二进制格式字符是允许输入的,但不是必需的(0,1)
>    所有的字母字符都大写
<    所有的字母字符都小写
!    关闭大小写转换
\    使用"\"转义上面列出的字符
```
