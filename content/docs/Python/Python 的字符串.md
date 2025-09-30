---
title: Python 的字符串
date: 2025-09-15T14:03:57+08:00
author: LiangMingJian
---

# 字符串的创建

在 Python 中，通过单引号，双引号和三引号包裹的数据会被识别为字符串（str）。

```python
str0 = "Hello"   # 双引号
str1 = 'World'  # 单引号

# 三引号
str2 = """123""" 
str3 = '''456'''
```

特别的，对于多行的字符串，可以使用斜杆 `\` 进行拼接展示。

```python
# 将多个字符串用斜杆从外部拼接
str4 = "Python 是一门易于学习、功能强大的编程语言。"\  
       "它提供了高效的高级数据结构，还能简单有效地面向对象编程。"  

# 将单个字符串用斜杆从内部拼接，注意第二行要顶格
str5 = "Python 是一门易于学习、功能强大的编程语言。\  
它提供了高效的高级数据结构，还能简单有效地面向对象编程。"
```

# 字符串的运算

在 Python 中，字符串支持使用 `+` 进行拼接，也支持使用 `*` 号进行重复。

```python
# 使用加号拼接
print('Hello'+' '+'World' )  # Hello World

# 使用星号重复
print('-' * 3)  # ---
```

# 字符串的转换

Python 提供内置函数 `str()` 用以将其他数据类型如整型，浮点型，字典，列表，元组等转换为字符串类型。

```python
print(str(1))  # 1
print(str(1.0))  # 1.0
print(str((1, 2, 3)))  # (1, 2, 3) 
print(str([1, 2, 3]))  # [1, 2, 3]
print(str({"Name": "Python"}))  # {'Name': 'Python'}
```

除了支持将上述数据类型转换为字符串外，Python 还支持将 ASCII 码或 Unicode 码直接转换为对应字符。

```python
# 将 ASCII 码进行转换
# \x 表示以十六进制标识数据
print('\x50\x79\x74\x68\x6F\x6E')  # Python

# 将 Unicode 码进行转换
# \u 16 位十六进制 4 码位的字符，如果是 \U 则是 32 位 8 码位字符
print('\u0050\u0079\u0074\u0068\u006F\u006E')  # Python

# 根据 Unicode 字符名称进行转换（大多数表情）
print('\N{Face with Tears of Joy}')  # 😂
```

> [ ASCII 表 ](https://www.runoob.com/w3cnote/ascii.html)
> [ ASCII Table ](https://www.ascii-code.com/)
> [ Unicode 符号表 ](https://symbl.cc/cn/unicode-table)
> [ Unicode® 码表 ](https://www.unicode.org/charts/charindex.html)

特别的，Python 提供一些用于控制输出格式或容易冲突字符的转义序列。

|转义序列|含意|
|---|---|
|`\\`|反斜杠（`\`）|
|`\'`|单引号（`'`）|
|`\"`|双引号（`"`）|
|`\a`|ASCII 响铃（BEL）|
|`\b`|ASCII 退格符（BS）|
|`\f`|ASCII 换页符（FF）|
|`\n`|ASCII 换行符（LF）|
|`\r`|ASCII 回车符（CR）|
|`\t`|ASCII 水平制表符（TAB）|
|`\v`|ASCII 垂直制表符（VT）|
|`\ooo`|八进制数 ooo 字符（\777）|
|`\xhh`|十六进制数 hh 字符（\FF）|

```python
print("It\'s Python")  # It's Python
print("Hello\nWorld")  # 分为两行打印 Hello 和 World
```

另外，可以使用 `r` 或 `R` 无视转义字符的使用。

```python
print(r"It\'s Python")  # 原样格式输出 It\'s Python
print(R"Hello\nWorld")  # 原样格式输出 Hello\nWorld
```

# 字符串是元组

在 Python 中，字符串可以看作是一个不可变的元组对象。

元组可以看成是元素不可变的一个列表，因此，列表所支持的方法，元组也基本都支持，显然，字符串也基本都支持。

## 下标访问

像列表一样，字符串支持通过下标访问单个字符，但要注意，访问的结果禁止修改（因为字符串是元组，不可变）。

```python
str0 = "Python"  

# 通过下标访问单个字符
print(str0[0])   # P
print(str0[-1])  # n

# 禁止像下面一样修改
str0[0] = "1"  # TypeError: 'str' object does not support item assignment
```

## 切片

像列表一样，字符串支持通过切片访问。

```Python
str0 = "Python"  
print(str0[0:3])      # Pyt
print(str0[-1:3:-1])  # no
```

> 详细的切片操作可查看：[ 如何使用 Python 进行列表切片 ](https://zhuanlan.zhihu.com/p/1941792644427654837)

## 查找

像列表一样，支持通过 in 和 not in 来判断某个元素是否在字符串里面。

```python
str0 = "Python"  
print('P' in str0)  # True  
print('p' not in str0)  # True
```

另外也支持 `index()` 方法，查找某个元素的位置。

```python
str0 = "Python"  
print(str0.index('P'))  # 0
```

特别的，字符串对于查找，还提供自有的方法 `find()`（失败时返回 -1，而不像 index 直接报错），`rfind()` 和 `rindex()`（从右开始查找），`endswith()`（是否以指定字符结尾）。

```python
str0 = "Python" 
print(str0.find('p'))      # -1
print(str0.rfind('P'))     # 0
print(str0.rindex('P'))    # 0
print(str0.endswith('n'))  # True
```

注意，上面的查找方法都是支持限定开始结束范围（包含开始，不包含结束）的，比如：`find('p', 0, 3)`，`index('P', 3, 5)`，`endswith('n', 0, 3)`。

# 字符串格式化

通过在字符串前添加标记 `f`，可以让字符串变成格式化字符串，支持将数据直接应用到字符串中。

```python
data0 = 123  
str0 = f"Python {data0}"  
print(str0)  # Python 123
```

> 详细内容可查看：[ 如何使用 Python f-string 格式化字符串 ](https://zhuanlan.zhihu.com/p/1944049461090289386)

# 字符串的方法

## 最大最小 max / min

获取字符串中最大 Ascii 码和最小 Ascii 码的字符。

```python
str0 = "0123456789"
print(max(str0), min(str0))  # 9 0
```

## 统计次数 count

统计字符串中某个字符或某个子字符串的出现次数。

```python
str0 = "0123456789"

# 在整个字符串中查找
print(str0.count("0"))   # 1
print(str0.count("01")) # 1

# 限定查找范围 [0, 2)
print(str0.count("5", 0, 2))  # 0
```

## 大小写转换 capitalize / upper / lower /  swapcase / title

**capitalize** 首字符大写，其他字符全部小写。

```python
print("python".capitalize())  # Python
print("pYTHon".capitalize())  # Python
print(" pYTHon".capitalize()) #  python，因为首字符是空格，导致全部小写
```

**upper** 全部大写，或 **lower** 全部小写。

```python
str0 = "Python"
print(str0.upper())  # PYTHON
print(str0.lower())  # python
```

**swapcase** 大小写反转，大写变小写，小写变大写。

```python
str0 = "Python"
print(str0.swapcase())  # pYTHON
```

**title** 标题化字符串，首字母大写，其余小写。

```python
str0 = "hello pyThon"
print(str0.title())  # Hello Python
```

## 填充并对齐 rjust / zfill

**rjust** 将字符串按输入长度返回，原字符串右对齐，空白位置使用指定内容填充。

**zfill** 是 rjust 的特化版本，默认以 0 进行填充。

```python
str0 = '1'  
print(str0.rjust(4, '0'))  # 0001
print(str0.rjust(4, '-'))  # ---1

# 特别的，当默认填充 0 时，可以使用 zfill
print(str0.zfill(4))  # 0001
```

## 移除前后空白 strip

从首尾移除包括 `space`（空格），`\f`（换页），`\n`（换行），`\r`（回车），`\t`（水平制表符），`\v`（垂直制表符）在内的空白符。

```python
str0 = ' Python\r\n'  
print(str0.strip())  # Python

# 特别的，可以传入字符从首尾移除指定字符
str1 = 'Python'  
print(str1.strip('Pn'))  # ytho，从首尾移除了 P 和 n
```

## 分割 partition / split / splitlines

**partition** 将字符串按指定的分隔符分割为 2 个子串，然后返回一个 3 元的元组（左边字符，分割符，右边字符）。

```python
str0 = "www.google.com"  
print(str0.partition("."))  # ('www', '.', 'google.com')  
print(str0.partition("1"))  # ('www.google.com', '', '') 没有则不分割 
print(str0.partition("google")) # ('www.', 'google', '.com') 以整个子串分割

# 特别的，提供 rpartition 从右往左进行分割
print(str0.rpartition("."))  # ('www.google', '.', 'com')
```

**split** 将字符串按指定的分隔符分割为 n 个子串，然后返回一个包含所有子串的列表。

split 支持传入分割符 sep 和分割数量 maxsplit 2 个参数。当 sep 不填时，以所有空白符作为分割。当 maxsplit 不填时，则尽可能的进行分割。

```python
str0 = "abcdefg 123456 python"  
print(str0.split())  # ['abcdefg', '123456', 'python']
print(str0.split(' ', 1))  # ['abcdefg', '123456 python']，只分割 1 次
print(str0.split('A'))   # ['abcdefg 123456 python']，没有则不分割
```

**splitlines** 将字符串按换行符进行分割，支持参数 keepends 用来控制输出结果是否携带换行符。

支持的换行符有：`\r`，`\n`，`\r\n`。

```python
str0 = "abcdefg\n123456\npython"  
print(str0.splitlines())  # ['abcdefg', '123456', 'python']
print(str0.splitlines(True))  # ['abcdefg\n', '123456\n', 'python']
```

## 替换 replace / translate

**replace** 使用指定字符串替换原有字符串，支持传入参数 count，控制替换次数。

```python
str0 = "python python python"  
print(str0.replace("python", "python3"))     # C C C
print(str0.replace("python", "python3", 1))  # C python python
```

**translate** 构建一个转换表，将字符串中所有字符进行逐个替换（常用于加密）。

```python
# 将 1/2/3 转换为 a/b/c，并删除 7/8/9
table = str.maketrans('123', 'abc', '789')  
str0 = '123456789'  
print(str0.translate(table))  # abc456
```

## 合并 join

**join** 将给定的可迭代对象按指定的字符进行连接。

特别注意，迭代对象必须是字符，不能是其他数据。

```python
data0 = ['1', '2', '3']  
data1 = ('4', '5', '6')  
  
print(''.join(data0))   # 123
print('-'.join(data1))  # 4-5-6
```

# 字符串的判断

Python 的字符串提供一些判断方法用来识别字符串的内容是否全是字母，是否全是数字，由或者字符串是否能被正确打印。

支持的方法有：

|方法|描述|
|---|---|
|`str.isalnum()`|所有字符都是字母或数字，并且至少有一个字符|
|`str.isalpha()`|所有字符都是字母，并且至少有一个字符|
|`str.isdigit()`|所有字符都是数字，并且至少有一个字符（罗马数字也是数字）|
|`str.isdecimal()`|所有字符都是十进制字符，并且至少有一个字符（只包括 0, 1 到 9）|
|`str.isnumeric()`|所有字符均为数值字符，并且至少有一个字符（所有在 Unicode 中设置数值属性的字符）|
|`str.isascii()`|字符串为空，或字符串中的所有字符都是 ASCII|
|`str.isprintable()`|所有字符均为可打印字符|
|`str.isspace()`|字符串中只有空白字符，并且至少有一个字符|
|`str.isidentifier()`|字符串是有效的标识符（能被 Python 识别为关键字或变量名，函数名的）|
|`str.islower()`|所有字符都是小写|
|`str.isupper()`|所有字符都是大写|
|`str.istitle()`|所有字符串都是首字母大写，其余小写|

```python
str0 = '123456'  
print(str0.isdigit())  # True
print(str0.isalpha())  # False
```

# 拓展阅读：字节流 Bytes

Python 3 中明确区分字符串类型 (str) 和字节流类型 (bytes)。

在计算机系统中，所有的数据都是以字节流形式保存的，它由一个一个 8 bit 字节（byte）按顺序组成。这些字节流经过解码会变成我们常见的字符串数据。

Python 3 对字节流 Bytes 数据要求使用带 b 前缀加字符串表示。

字节流 Bytes 的字符串要求：如果数据值在 Ascii 码值内，则可以直接写入对应的 Ascii 字符，反正，则必须输入一个 \x 16 进制的字节值。

```python
str0 = b'Python'  
print(str0)  # b'Python'
  
str1 = b'\xe6\x88\x91'  
print(str1)  # b'\xe6\x88\x91'
```

对于字节流 Bytes 数据，在知道其解码格式后，便可以使用 decode 方法将字节流转换为用户可读的数据。

```python
str0 = b'Python'  
print(str0.decode('Ascii'))  # Python 
  
str1 = b'\xe6\x88\x91'  
print(str1.decode('UTF-8'))  # 我
```

当然，我们也可以将字符串按指定编码格式使用 encode 或 bytes 方法转换为字节流。

```python
str0 = '我'  
print(str0.encode('UTF-8'))  # b'\xe6\x88\x91'
print(bytes(str0, 'UTF-8'))  # b'\xe6\x88\x91'
```

字节流 Bytes 数据可以通过 hex 方法和 fromhex 方法存储为一个 16 进制字符串和从 16 进制字符串中恢复，实现数据保存。

```python
# 使用 hex 保存为 16 进制字符串
str0 = b'\xe6\x88\x91'  
data = str0.hex()  
print(data)  # e68891

# 从 16 进制字符串恢复
str1 = bytes.fromhex(data)  
print(str1)  # b'\xe6\x88\x91'
```

字节流 Bytes 数据是只读的，禁止修改，如果需要一个可读写的对象，可以使用 bytearray 方法进行创建，其功能与 Bytes 一致。

```python
data = bytearray(b'\xe6\x88\x91')  
print(data)  # bytearray(b'\xe6\x88\x91')
data[0] = 0x88  
print(data)  # bytearray(b'\x88\x88\x91')
```
