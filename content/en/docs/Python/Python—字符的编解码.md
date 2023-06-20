---
title: Python 字符的编解码
date: 2021-07-23
author: LM
---

## 1.字符的存储

在计算机中所有字符都是以二进制代码储存的，系统根据不同的编码格式，将二进制代码转换为字符显示。

```python
print("\u0394")
print("\U00000394")
print("\N{greek capital letter delta}")
```

## 2.乱码时的处理

当编码格式与解码格式出现冲突时就会出现乱码，而此时往往需要将报错的部分进行替换处理，使得程序整体不出错。

```python
# 不处理
print((b"\x80abc").decode("utf-8","strict"))
# 替换为U+FFFD
print((b"\x80abc").decode("utf-8","replace"))
# 加上反斜杠
print((b"\x80abc").decode("utf-8","backslashreplace"))
# 直接忽略
print((b"\x80abc").decode("utf-8","ignore"))
```

## 3.字符与二进制的转换

### a.二进制转换字符

```python
u=chr(40960)+"abce"+chr(1972)
print(u)
u1=chr(123)
print(u1)
```

### b.字符转换二进制

```python
u="中国abc"
print(u.encode("utf-8"))
# b'\xe4\xb8\xad\xe5\x9b\xbd'
print(u.encode("ascii"))
print(u.encode("ascii","ignore"))
print(u.encode("ascii","replace"))
print(u.encode("ascii","xmlcharrefreplace"))
print(u.encode("ascii","backslashreplace"))
print(u.encode("ascii","namereplace"))
```