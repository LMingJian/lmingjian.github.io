---
title: Python 的正则处理
date: 2025-09-25T09:47:41+08:00
author: LiangMingJian
---

# 前言

在 Python 中，通过标准库 `re`，开发者可以完成对字符串的正则处理。可以发现，标准库 `re` 就是正则表达式（Regular Expression）的缩写。

> 正则的使用可查阅另一篇文章：[ 正则使用手册 ](https://zhuanlan.zhihu.com/p/1952010198693687619)

# 查找所有匹配的结果

在 `re` 模块中，通过方法 `findall()` 和 `finditer()` 可以返回目标字符串中按给定正则表达式匹配的所有结果子串。

> `findall()`

`findall()` 接收一个正则表达式参数（需要使用 `r` 标记避免转义）与一个目标字符串参数，然后根据正则表达式，返回所有匹配子串数据的列表。

```python
import re  

target = "Hello Python"

# 查找目标中所有非数字的单个子符
res = re.findall(r"\D", target)  
print(res)
# ['H', 'e', 'l', 'l', 'o', ' ', 'P', 'y', 't', 'h', 'o', 'n']

# 查找目标中所有数字的单个子符
res = re.findall(r"\d", target)  
print(res)
# []
```

> `finditer()`

`finditer()` 与 `findall()` 类似，同样接收一个正则参数与一个目标参数，但 `finditer()` 会返回一个匹配对象 Match。

匹配对象 Match 与普通的字符串结果相比，具有更强大的功能，可以在正则匹配后完成一些更复杂工作（其详细功能在下文后半部分进行介绍）。

```python
import re  
  
target = "Hello Python"  
  
# 查找目标中所有非数字的单个子符  
res = re.finditer(r"\D", target)  
for match in res:  
    print(match.group())
# match.group() 打印匹配的完整结果
# 逐个打印目标字符串的字符
```

# 查找单个匹配的结果

在 `re` 模块中，通过方法 `match()` 和 `search()` 可以在目标字符串中搜索匹配结果，与 `findall()` 等不同，这两个方法一旦匹配成功就会立即返回，即匹配结果只会是一个。

> `search()`

search 会扫描整个整个目标字符串，然后根据正则表达式匹配的第一个位置返回相应的匹配对象 Match。 如果字符串中没有与表达式匹配的位置则返回 None。

```python
import re  
  
target = "123Hello Python"  
  
# 查找目标中第一个非数字的单个子符
res = re.search(r'\D', target)  
print(res.group())
# 输出结果 H
```

> `match()`

match 只会扫描开头的字符串，即从第 1 个字符开始，如果存在与正则表达式相匹配的内容则返回匹配对象 Match，如果不存在则返回 None。

```python
import re  

target1 = "123Hello Python"  
target2 = "Hello Python"  

# 查找目标开头中第一个非数字的单个子符
res = re.match(r'\D', target1)  
print(res)
# 因为 target1 开头就是数字，所以无法匹配
# 输出结果 None

# 查找目标开头中第一个非数字的单个子符  
res = re.match(r'\D', target2)  
print(res)
print(res.group())
# 因为 target2 开头就是字母，所以可以匹配
# 输出结果 <re.Match object; span=(0, 1), match='H'>
# 输出结果 H
```

# 完全匹配

> `fullmatch()`

完全匹配常常用于检查多个字符串是否都符合一定格式，比如都是字母，都是数字等。

fullmatch 要求整个目标字符串必须与正则表达式所匹配，如果不匹配则返回 None。

```python
import re  
  
target1 = "Hello"  
target2 = "Hello123"  

# 匹配 5 长度的非数字字符
res = re.fullmatch(r'\D{5}', target1)  
print(res)
# target1 就是 5 长度的非数字字符，所以可以匹配
# <re.Match object; span=(0, 5), match='Hello'>

# 匹配 5 长度的非数字字符
res = re.fullmatch(r'\D{5}', target2)  
print(res)
# target2 不是 5 长度的非数字字符，所以不可以匹配
# None
```

# 分组匹配

在正则中，括号 `()` 包裹的表达式被称为分组，分组的内容在匹配时会单独作为一个结果进行展示，没有分组的内容将不会作为结果，比如下述匹配：

```python
target = 'AB BC CD'  
  
# 不分组  
res = re.findall(r'\w\w', target)  
print(res)
# 两两字符作为一个结果输出
# 结果为 ['AB', 'BC', 'CD']

# 分组
res = re.findall(r'(\w)\w', target)  
print(res)
# 将两两字符匹配结果中的分组内容输出，另外一个不输出
# 结果为 ['A', 'B', 'C']
```

显然，分组操作能让我们从目标字符串中提取我们需要的内容。

在 Python 中，除了上述使用括号支持的分组操作外，还提供一些 Python 特有的分组功能。

|分组操作|描述|
|---|---|
|(exp)|匹配表达式，并自动编号（从左到右，从 1 开始）|
| \\id |在表达式中引用编号为 id 的内容，例如 (\\d)abc\\1 等价于 (\\d)abc(\\d)|
|(?P\<name\>exp)|为分组命名，例如 (?P\<name\>\\d)，(\\d) 这个分组被命名为 name|
|(?P=name)|引用命名为 name 的分组，例如 (?P\<name\>\\d)abc(?P=name) 等价于 (?P\<name\>\\d)abc(?P\<name\>\\d)|

需要注意，上述的引用的不是表达式，而是匹配结果，即引用后的匹配结果与原表达式一致，而不是重新匹配。

```python
import re  
  
target = "1abc1 2abc3"
  
# 编号引用 
res = re.findall(r'(\d)abc\1', target)  
print(res)
# 结果只有 ['1']
# 因为 2abc3 的 2 和 3 不一致，引用表达式结果与原表达式不一样

# 命名引用
res = re.findall(r'(?P<num>\d)abc(?P=num)', target)  
print(res)
# 结果 ['1']
```

此外，Python 分组还支持位置匹配，用于指定一个位置。

|分组操作|描述|
|---|---|
|(?=exp)|匹配exp字符串前的位置|
|(?<=exp)|匹配exp字符串后的位置|
|(?!exp)|不匹配exp字符串前的位置|
|(?<!exp)|不匹配exp字符串后的位置|

```python
import re  

# 对下面字符串，有位置 ^0^a^b^c^1^
target = "0abc1"
  
# (?=abc) 定位 ^a 前面的 1 个位置，然后从该位置开始匹配一个字符
res = re.findall(r'(?=abc).', target)  
print(res)
# ['a']

# (?<=abc) 定位 c^ 后面的 1 个位置，然后从该位置开始匹配一个字符
res = re.findall(r'(?<=abc).', target)  
print(res)
# ['1']

# (?!abc) 除了 ^a 前的位置，其他位置都支持匹配一个字符
res = re.findall(r'(?!abc).', target)  
print(res)
# ['0', 'b', 'c', '1']

# (?<!abc) 除了 c^ 后的位置，其他位置都支持匹配一个字符
res = re.findall(r'(?<!abc).', target)  
print(res)
# ['0', 'a', 'b', 'c']
```

# 分割匹配

> `split()`

split 通过正则表达式的使用，将正则表达式匹配的内容作为分割标记，将目标字符串分割为一个列表。

```python
import re  
  
target = "1, 2`·3.4 5-6"  
  
# 以 \W+ 任意个非字母数字下划线字符为标记划分字符串  
res = re.split(r'\W+', target)  
print(res)
# ['1', '2', '3', '4', '5', '6']
```

# 替换匹配

> `sub()`

sub 将正则表达式匹配的内容替换为指定内容，支持传入 count 参数来控制替换次数，默认全部替换。

特别的，存在兄弟方法 `subn()` 在返回结果外，还返回对应的替换次数。

```python
import re

target = "1, 2`·3.4 5-6"  
  
# 以 \W+ 任意个非字母数字下划线字符为标记替换字符串  
res = re.sub(r'\W+', ', ', target)  
print(res)
# 1, 2, 3, 4, 5, 6

# 只替换 2 次
res = re.sub(r'\W+', ', ', target, count=2)  
print(res)
# 1, 2, 3.4 5-6

# 兄弟方法 subn()
res = re.subn(r'\W+', ', ', target)  
print(res)
# 元组 (替换结果, 替换次数) = ('1, 2, 3, 4, 5, 6', 5)
```

# 复用正则表达式

> `compile()`

在 `re` 模块中，通过 `compile()` 方法可以将一个正则表达式字符串编译为一个正则表达式对象。这个正则表达式对象可以在所有匹配方法中进行复用，同时支持设置匹配模式。

`re` 模块支持 6 种匹配模式，在 `compile()` 方法中可以使用位或运算符 `|` 同时使用多个模式：

1. **re.I（re.IGNORECASE）**：匹配时忽略大小写。
2. **re.M（re.MULTILINE）**：改变 `^` 和 `$` 的匹配行为，可以匹配任意一行的行首和行尾。
3. **re.S（re.DOTALL）**：`.` 可以匹配任意字符，包含 `\n`。
4. **re.L（re.LOCALE）**：使基本元字符的匹配逻辑取决于当前区域设定。
5. **re.U（re.UNICODE）**：使基本元字符的匹配逻辑取决于 Unicode 定义。
6. **re.X（re.VERBOSE）**：详细模式，正则表达式可以写成多行，忽略空白字符，并支持注释。

```python
import re  
  
target = "Hello Python"  
# 编译正则表达式对象
pattern = re.compile(r"\D", re.M | re.VERBOSE)  
  
res = re.findall(pattern, target)  
print(res)
```

# 匹配对象 Match

匹配对象 Match 是由 `match()` 和 `search()` 方法所返回的，它是比字符串结果更高级的一种匹配内容，能比字符串结果提供更多的操作。

> `Match.expand(template)`

将模板字符串 template 中的分组引用内容替换为实际匹配内容，支持数字分组索引（`\1, \2`）和命名分组索引（`\g<name>`）。

```python
import re  
  
target = 'Hello Python'  

# 创建一个分组匹配内容
res = re.match(r'(?P<p1>\w+) (?P<p2>\w+)', target)  

# 将模板内容替换为实际匹配内容
print(res.expand(r'Data: \g<p1> \g<p2>'))  
print(res.expand(r'Data: \1 \2'))
# 输出 Data: Hello Python
```

> `Match.group()`

将匹配结果中的内容进行返回。如果匹配内容是分组的，则可以传入 0 到 N（0 或者空返回整个匹配内容），将分组匹配结果进行返回。另外，支持命名分组的索引调用。

特别的，存在兄弟方法 `groups()` 用以直接返回一个包含所有匹配的元组。

```python
import re  
  
target = 'Hello Python'  
  
# 创建一个分组匹配内容  
res = re.match(r'(?P<p1>\w+) (?P<p2>\w+)', target)  
  
# 0 或者空时，返回整个匹配内容
print(res.group())  
print(res.group(0))
# Hello Python

# 分组匹配时，返回第 1 组和第 2 组内容
print(res.group(1))  
print(res.group(2))
# Hello 和 Python

# 支持多参数使用，返回一个结果元组
print(res.group(1, 2)) 
# ('Hello', 'Python')

# 支持命名索引调用
print(res.group('p1'))  
print(res.group('p2'))
# Hello 和 Python 

# 返回一个包含所有匹配结果的元组
print(res.groups())
# ('Hello', 'Python')
```

> `Match.groupdict()`

当匹配内容存在命名分组时，可以通过 groupdict 将匹配结果以字典形式返回。

```python
import re  
  
target = 'Hello Python'  
  
# 创建一个分组匹配内容  
res = re.match(r'(?P<p1>\w+) (?P<p2>\w+)', target)  

# 以字典返回
print(res.groupdict())
# {'p1': 'Hello', 'p2': 'Python'}
```

————————————

> [ re --- 正则表达式操作 ](https://docs.python.org/zh-cn/3/library/re.html)
