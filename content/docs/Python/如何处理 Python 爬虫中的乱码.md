---
title: 如何处理 Python 爬虫中的乱码
date: 2024-12-25T16:08:47+08:00
author: LiangMingJian
---

# 为什么会出现乱码

在使用 Python 爬虫时，有些网站获取的数据会出现乱码，这是因为 Requests 模块在获取到响应结果的文本后，会基于 HTTP 头对响应的编码作出推测，当推测的编码格式错误时，文本就会出现乱码。

我们可以使用以下这两个方法查看响应文本的编解码类型。

```python
print(res.encoding) 
# 查看网页返回的字符集类型，其值是从 header 中的 charset 字段中提取的编码方式，若 header 中没有 charset 字段则默认为 ISO-8859-1 编码模式。
print(res.apparent_encoding) 
# 自动判断字符集类型，apparent_encoding 会从网页的内容中分析网页编码的方式，当网页出现乱码时可以把 apparent_encoding 的编码格式赋值给 encoding。
```

输出结果

```python
# Python 使用的编解码格式
ISO-8859-1
# 实际应该使用的编解码格式
GB2312
```

# 乱码怎么出现的

当源网页编码和爬取下来后的编码转换不一致时，程序就会出现乱码。

比如源网页为 GBK 编码的字节流，在我们抓取后，程序直接使用 UTF-8 进行编码并输出到文件中，此时必然会引起乱码。

# 主流的编码格式有哪些

现如今主流的编码有：Unicode、ISO-8859-1、GBK2312、GBK、UTF-8 等几种。

最早的编码是 ISO-8859-1，和 ASCII 编码相似。ISO-8859-1 属于单字节编码，最多能表示的字符范围是 0-255，应用于英文环境。很明显，ISO-8859-1 编码表示的字符范围很窄，无法表示中文字符。

1981 年中国通过对 ASCII 编码的扩充改造，产生了 GBK2312 编码，可以表示 6000 多个常用汉字。但由于汉字实在是太多了，包括繁体和各种字符，于是产生了 GBK 编码，它包括了 GBK2312 中的编码，同时扩充了很多。

随着时代的发展，其他国家都像中国一样，把自己的语言进行了编码，出现了很多的编码格式。终于，有个叫 ISO 的组织看不下去了。他们创造了一种编码 Unicode，这种编码非常大，大到可以容纳世界上任何一个文字和标志。因此，只要电脑上有 Unicode 这种编码系统，无论是全球哪种文字，只要是 Unicode 编码的，都可以被其他电脑正常解释。Unicode 在网络传输中，出现了两个标准 UTF-8 和 UTF-16，分别每次传输 8 个位和 16 个位。

# 什么是编码

可以这样来理解：字符串是由字符构成，字符在计算机硬件中通过二进制形式存储，这种二进制形式就是编码。如果直接使用`字符串>字符>二进制表示（编码）`，会增加不同类型编码之间转换的复杂性。所以引入了一个抽象层`字符串>字符>与存储无关的表示>二进制表示（编码）`，这样，可以用一种与存储无关的形式表示字符，不同的编码之间转换时可以先转换到这个抽象层，然后再转换为其他编码形式。在这里，Unicode 就是与存储无关的表示，UTF-8 就是二进制表示。

# 如何解决乱码

1. 直接指定 `res.encoding`

   ```python
   import requests
   url = "http://search.51job.com"
   res = requests.get(url)
   res.encoding = "GBK"
   html = res.text
   print(html)
   ```

2. 通过 `res.apparent_encoding` 属性指定

   ```python
   import requests
   url = "http://search.51job.com"
   res = requests.get(url)
   res.encoding = res.apparent_encoding
   html = res.text
   print(html)
   ```

3. 通过编码、解码的方式

   ```python
   import requests
   url = "http://search.51job.com"
   res = requests.get(url)
   html = res.text.encode('ISO-8859-1').decode('GBK')
   print(html)
   ```
