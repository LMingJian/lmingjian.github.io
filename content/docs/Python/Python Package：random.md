---
title: Python Package：random
date: 2024-12-25T16:12:44+08:00
author: LiangMingJian
---

# 概述

> 标准库

random 是 Python 中用于生成各种**伪随机数**的一个模块，其支持随机整数，随机序列元素抽取，随机列表排列等功能。

需要注意，random 生成的随机数是伪随机数，是存在一定规律的，只是看起来随机而已。如果要提高安全性，则建议使用 secrets 模块（见下文拓展阅读），生成安全的随机数据。

# 随机返回一个给定范围的整数

`random.randint(a, b)`，返回的整数满足 `a <= N <= b`。

```python
import random

print(random.randint(1, 100))
```

# 随机返回一个 0 到 1 的浮点数

`random.random()`，返回满足 `0.0 <= N < 1` 的随机浮点数，不包括 1。

```python
import random

print(random.random())
```

# 随机返回一个给定范围的浮点数

`random.uniform(a, b)`，当 `a <= b` 时，返回满足 `a <= N <= b` 的浮点数，当 `d < a` 时，返回满足 `b <= N <= a` 的浮点数，即取值范围是传入参数小到大，不看位置。

```python
import random

# 下述取值范围一致
print(random.uniform(1, 2))
print(random.uniform(2, 1))
```

# 随机从序列返回一个元素

`random.choice(seq)`，接收一个非空序列，**可以是列表，元组**，并随机返回序列中的一个元素。特别的，字符串在 Python 中也可以看成一个由多个字符组成的序列，所以在这里也可以传入。

```python
import random  
  
x = [1, 2, 3, 4, 5, 6]  
y = (1, 2, 3, 4, 5, 6)  
z = 'abcdefg'
  
print(random.choice(x))  
print(random.choice(y))
print(random.choice(z))  # 输出：b 或其他单个字符
```

# 随机从序列抽取多个元素

`random.choices(population, weights=None, cum_weights=None, k=1)`，从给定序列 population 中抽取 k 个元素，并**返回为一个长度 k 的列表，数据允许重复**。

weights 和 cum_weights 是一个列表参数，其长度要与 population 相等，功能都是代表权重，其中 cum_weights 是累积权重。

比如对于一个长度 5 的序列，当 weights 等于 `[1, 5, 2, 3, 4]` 时，表示对应第 1 至第 6 个元素抽取概率为 `1/(1+5+2+3+4)=1/15, 5/15, 2/15...`。如果换成 cum_weights 则应当写成 `[1, 6, 8, 11, 15]`，概率结果与 weights 一致，只是将权重累加而已。

```python
import random  
  
x = [1, 2, 3, 4, 5, 6]  
  
print(random.choices(x, k=3))  
# 输出：[2, 6, 6]
print(random.choices(x, weights=[1, 5, 1, 1, 1, 1], k=3))
# 输出：[2, 2, 2]，因为第 2 个权重很高，所以抽取概率大
```

# 随机从序列抽取多个不同的元素

`random.sample(population, k, counts=None)`，从给定序列 population 中抽取 k 个元素，并**返回为一个长度 k 的列表，元素不允许重复**。

与 `choices()` 类似，但 `sample()` 在抽取成功后不会将元素放回，比如对一个 5 位序列抽取第一次后，第二次抽取将会是 4 位，因为第一次抽取的结果会被剔除。

显然，这个的元素不重复不是指数据不重复，如果原序列中存在重复的数据，那抽取后的结果仍然会有重复数据，`sample()` 只是保证抽取的元素在原序列的位置是唯一的。

对应原序列的重复元素，可以手动一个一个列出，比如 `[1, 1, 1, 2, 2]`，或者在调用 `sample()` 时传入 counts 自动扩展。比如 `sample([1, 1, 1, 2, 2])` 就等价于 `sample([1, 2], counts=[3, 2])`。

```python
import random  
  
x = [1, 2, 3, 4, 5, 6]  
y = [1, 2]
  
print(random.sample(x, k=3))
# 输出：[6, 1, 5]
print(random.sample(y, k=5, counts=[3, 2]))
# 因为使用 counts 扩展，所以能抽取比原序列长度更多的数据
```

# 随机返回一个 range 列表中的数据

`random.randrange(start, stop[, step])`，先使用 `range(start, stop, step)` 生成一个列表，然后随机从中取值，返回的数据满足 `start <= N < stop, N - n * step = start`。

```python
# 随机从列表 [1, 100] 步长 1 中挑取数据
print(random.randrange(1, 100))

# 随机从列表 [1, 100] 步长 2 中挑取数据
print(random.randrange(1, 100, 2))
```

# 随机打乱一个序列

`random.shuffle(x)`，随机打乱传入的可变对象序列，然后**返回打乱后的原序列**。

```python
import random  
  
x = [1, 2, 3, 4, 5, 6]  
random.shuffle(x)  
  
print(x)
```

# 拓展阅读：使用 secrets 生成安全的随机数

## 安全的从序列中抽取元素

`secrets.choice(seq)`，从一个非空序列中随机返回一个元素。

```python
import secrets  
  
x = [1, 2, 3, 4, 5, 6]  
  
print(secrets.choice(x))
```

## 安全的返回一个整数

`secrets.randbelow(bound)`，返回一个 `0 <= N < bound` 的整数。

```python
import secrets  
  
print(secrets.randbelow(100))
```

## 安全的生成一个十六进制随机 Token

`secrets.token_hex([nbytes=None])`，生成一个具有 nbytes 个随机字节的十六进制随机文本字符串，每个字节转换为两个十六进制数码，即 nbytes 等于 2 时，字符串长度 4，等于 16 时，字符串长度 32。

```python
import secrets  
  
print(secrets.token_hex(16))
# 输出：9c62433721990c8bb9e344f94902b442
```

## 安全的生成一个随机 URL Token

`secrets.token_urlsafe([nbytes=None])`，生成一个具有 nbytes 个随机字节的 URL 文本字符串，文本使用 Base64 编码，每个字节转换约 1.3 个字符，即 nbytes 等于 2 时，字符串长度 3，等于 16 时，字符串长度 22。

```python
import secrets  
  
print(secrets.token_urlsafe(16))
# 输出：BfSlvP8kKaqP9RtrPtf8WQ
```

————————————

> [ random 官方文档 ](https://docs.python.org/zh-cn/3.13/library/random.html)

> [ secrets 官方文档](https://docs.python.org/zh-cn/3.13/library/secrets.html)
