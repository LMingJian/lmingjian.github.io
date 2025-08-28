---
title: 如何使用 Python 进行 URL 编解码
date: 2024-12-25T16:09:16+08:00
author: LiangMingJian
---

# 需求

在使用 Python 爬取网页时，某些时候可能需要保存访问的 URL 地址，并对这些 URL 地址进行分析。但由于 URL 的设计，包括空格、中文、标点符号等特殊字符都会被进行编码，因此我们爬取的内容无法被识别分析。此时，如何将该内容正确解码就成了一个问题。

比如：【我爱 Python】在 URL 编码后会变成【%e6%88%91%e7%88%b1+Python】。

# 通过 unquote 或 unquote_plus 方法解码

unquote，unquote_plus 都是 urllib.parse 模块中用于 URL 解码的一种方法。

> urllib.parse 是 Python 标准库中用于解析和操作 URL 的模块，其主要功能包括： ‌URL 编码/解码‌、‌解析 URL 结构‌、‌拼接/拆分 URL‌ 等。（相关资料：[urllib.parse 文档](http://link.zhihu.com/?target=https%3A//docs.python.org/zh-cn/3.13/library/urllib.parse.html)）

unquote，unquote_plus 两者在使用过程的区别在于对空格和 + 号的解析处理。

- unquote：`%20` 会被解析为空格，`+` 会被解析为加号。
- unquote_plus： `%20` 会被解析为空格，`+` 不会被解析，仍保持原状。

比如前言中的示例 `%e6%88%91%e7%88%b1+Python`，解码后：

```python
from urllib.parse import unquote, unquote_plus

encoded1 = unquote('%e6%88%91%e7%88%b1+Python')
encoded2 = unquote_plus('%e6%88%91%e7%88%b1+Python')
print(f'encoded1: {encoded1}')
print(f'encoded2: {encoded2}')
```

执行结果：

```
encoded1: 我爱+Python
encoded2: 我爱 Python
```

# 通过 quote 或 quote_plus 方法编码

quote ，quote_plus 都是 urllib.parse 模块中用于 URL 编码的一种方法。

quote ，quote_plus 两者区别同样在于对空格的处理：

- quote：将空格转换为 `%20`。
- quote_plus：将空格转换为 `+`。

比如前言中的示例 `我爱 Python`，编码后：

```python
from urllib.parse import quote, quote_plus

decoded1 = quote('我爱 Python')
decoded2 = quote_plus('我爱 Python')
print(f'decoded1: {decoded1}')
print(f'decoded2: {decoded2}')
```

执行结果：

```
decoded1: %E6%88%91%E7%88%B1%20Python
decoded2: %E6%88%91%E7%88%B1+Python
```
