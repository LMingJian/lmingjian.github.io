---
title: 如何使用 Python 对字符进行编解码
date: 2024-12-25T16:09:34+08:00
author: LiangMingJian
---

# 前言

在计算机中，所有的字符都是以二进制代码存储的。计算机系统根据不同的编码格式，将二进制代码转换为相应的字符。

在 Python 中，我们可以通过 encode 和 decode 来对这些代码或字符进行编解码。

# 使用 encode 将字符编码

```python
string = "我爱 Python"

print(string.encode("utf-8"))
```

上述代码在执行后会输出 我爱 Python 的 UTF-8 编码： `b'\xe6\x88\x91\xe7\x88\xb1 Python'`。

上述结果中，b 表示是一个字节串，而不是一个字符，其存储的是字符的原有二进制数值。

需要注意，Python 的字节串默认将中文以转义序列 `\x??` 这种格式表示，`\x` 表示使用 2 位十六进制。而对于英文等 ASCII 字符，Python 往往会在输出时自动翻译。

我们可以遍历字节串，然后对每个十六进制代码通过字符格式化方法 `format(byte, '08b')` 转换为实际的二进制代码。

```python
string = "我爱 Python"

for byte in string.encode("utf-8"):
    print(' '.join(format(byte, '08b')))
```


上述代码执行结果：`11100110 10001000 10010001 11100111 10001000 10110001 00100000 01010000 01111001 01110100 01101000 01101111 01101110`。

可以看到，其结果正是 2 位十六进制数据。这一串二进制数据就是 我爱 Python 这一字符串以 UTF-8 格式在计算机系统中的存储内容。

# 使用 decode 将字符解码

```python
code = b'\xe6\x88\x91\xe7\x88\xb1 Python'

print(code.decode('utf-8'))
```

decode 能将字节串按目标编码转换为实际的字符。比如上述代码，就将字节串 code 转换回 我爱 Python。

# 如何处理无效字节（乱码）

在编码解码时，有时候因为格式错误，字符串可能会携带一些无法被正确识别的字节，这种字节往往会导致乱码。此时我们可以在 encode 和 decode 时，通过传递 errors 参数，将这些无效字节进行处理。

```python
code1 = b'\xe6\x88\x91\xe7\x88\xb1'
code2 = b'\xe6\x88\x91\xe7\x88\xff'   # 包含无效字节 \xff

print(code1.decode('utf-8', errors='replace'))
print(code2.decode('utf-8', errors='replace'))
```

正如上述示例代码，当系统碰到无法解析的字节时，会使用特殊字符替换，然后继续执行。通过这样处理，即使出现异常无效字节，也不会打断程序的正常执行。

encode 和 decode 都支持以下 3 种错误字节的处理方法：

- ignore：忽略错误字节，返回空白。
- replace：使用特殊的占位符替换无效字节。
- backslashreplace：使用转移序列如 \xff 表示无效字节，即将无效字节直接打印出来。
