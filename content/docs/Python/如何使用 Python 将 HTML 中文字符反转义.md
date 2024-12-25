---
title: 如何使用 Python 将 HTML 中文字符反转义
date: 2024-12-25T16:09:16+08:00
author: LiangMingJian
---

# 需求

在爬取 HTML 中，部分中文与符号需要进行转义才能显示真实的内容，然后进行储存。

# 实现

```python
import html
from urllib.parse import urlparse
from urllib.parse import urljoin
from urllib.parse import urlencode, parse_qs, parse_qsl
from urllib.parse import quote, unquote

__author__ ='Evan'
print('返回⼀个ParseResult类型的对象: ', urlparse('http://www.baidu.com/index.html;user?id=5#comment'))
print('合并两个字符串组合成⼀个完整的URL: ', urljoin('http://www.baidu.com','index.html'))
params = {'name':'Evan', 'id':'77'}
print('将字典序列化为Get请求参数: ','http://www.baidu.com?'+ urlencode(params))
print('将Get请求参数反序列化为字典: ', parse_qs('http://www.baidu.com?name=Evan&id=77'))
print('将Get请求参数反序列化为列表: ', parse_qsl('http://www.baidu.com?name=Evan&id=77'))
print('将中⽂转化为URL编码: ','http://www.baidu.com?'+ quote('年龄'))
print('将URL编码转化为中⽂: ', unquote('http://www.baidu.com?%E5%B9%B4%E9%BE%84'))
print('HTML格式反转义成字符: ', html.unescape('https&#x3a;&#x2f;&#x2f;127.0.0.1&#x2f;report')
```

{{< details "参考文件" >}} 
1：[ Python爬虫之转义和反转义使用方法 ](https://wenku.baidu.com/view/4a893f47deccda38376baf1ffc4ffe473368fd80.html)
{{< /details >}}
