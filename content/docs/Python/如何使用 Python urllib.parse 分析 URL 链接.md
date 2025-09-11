---
title: 如何使用 Python urllib.parse 分析 URL 链接
date: 2025-08-20T14:26:01+08:00
author: LiangMingJian
---

# 前言

urllib.parse 支持将统一资源定位符 URL 字符串自动识别并拆分为不同部分，如协议、路径、参数等，或将各个部分组合回 URL 字符串。

urllib.parse 不仅支持 `http`，`https` 这类超文本网络协议，还支持如：`file`，`ftp`，`rtsp`， `sip`，`svn`，`telnet`，`ws`，`wss` 等互联网协议。

# 将 URL 解析为 6 个属性

`urllib.parse.urlparse()` 支持将一个 URL 解析为一个包含 6 个元素的具名元组（支持使用名字调用参数的元组）。

支持的解析属性包括：

|属性|索引|值|值（如果不存在）|
|---|---|---|---|
|`scheme`|0|URL 协议说明符|空字符串|
|`netloc`|1|网络位置部分（域名）|空字符串|
|`path`|2|分层路径（路由）|空字符串|
|`params`|3|路径参数（以分号 `;` 分隔的路径参数）|空字符串|
|`query`|4|查询组件（`?` 后的请求参数）|空字符串|
|`fragment`|5|片段标识符（`#` 后的请求参数）|空字符串|
|`username`|无|用户名（常见于 RTSP，FTP 等）|None|
|`password`|无|密码（在域名前用 `@` 分隔）|None|
|`hostname`|无|主机名（小写）|None|
|`port`|无|端口号为整数（如果存在）| None |

需要注意，现代路径参数往往使用路径变形 `/path/{id}`，不再使用 `;`，因此对于现代 URL，params 往往是没有值的。

**特别的，对于现代 URL，可以使用 `urlsplit()` 实现与 `urlparse()` 类似的功能**。

`urlsplit()` 相比  `urlparse()` 恰恰少的就是 `params` 分号参数的识别，其将分号参数合并到 `path` 参数中。

示例代码如下：

```python
import urllib.parse as urlparse  

# 解析常规数据
data1 = urlparse.urlparse("https://example.com/search;param=value"  
                          "?q=Windows+10&qs=MB#Win")

print(data1)
# ParseResult(scheme='https', netloc='example.com', path='/search',
# params='param=value', query='q=Windows+10&qs=MB', fragment='Win')

# 对于返回的具名数组，可以使用属性名称调用
print(data1.netloc)  # example.com

# 对于返回的具名数组，可以使用对应索引调用 
print(data1[1])      # example.com

# 解析用户名，密码，主机，端口
data2 = urlparse.urlparse("rtsp://admin:123456@192.168.1.64:554"  
                          "/Streaming/Channels/101")
print(data2.username)  # admin
print(data2.password)  # 123456
print(data2.hostname)  # 192.168.1.64
print(data2.port)      # 554

# 以 urlsplit 分析 URL
data3 = urlparse.urlsplit("https://example.com/search;param=value"    
                          "?q=Windows+10&qs=MB#Win")  
  
print(data3)
# SplitResult(scheme='https', netloc='example.com',
# path='/search;param=value', query='q=Windows+10&qs=MB', fragment='Win')
```

# 解析 URL 中的字符串参数

对于格式为 `?name=Tom&age=13&name=Jack` 这类 URL 参数，`urllib.parse` 提供 `parse_qs()` 和 `parse_qsl()` 两种方法供用户使用。

这两种方法的区别在于：`parse_qs()` 返回字典形式数据，字典的键唯一，对应参数名；`parse_qsl()` 返回元组形式数据，每一个参数对都是一个元组值。

比如下述代码：

```python
import urllib.parse as urlparse  
  
query = "q=Windows+10&qs=MB&q=A+C"  

# 以字典形式返回数据
data1 = urlparse.parse_qs(query)  
print(data1)  
# {'q': ['Windows 10', 'A C'], 'qs': ['MB']}
# 这里返回的参数会自动合并相同参数名的值，同时自动进行 URL 解码

# 以元组形式返回数据
data2 = urlparse.parse_qsl(query)  
print(data2)
# [('q', 'Windows 10'), ('qs', 'MB'), ('q', 'A C')]
# 这里返回的参数不会自动合并，但也会自动 URL 解码
```

# 合并参数到 URL 中

`urllib.parse` 提供与  `urlunparse()` 与 `urlunsplit()` 供开发者合并参数与路由为一个标准的 URL 字符串。

`urlunparse()` 与 `urlunsplit()` 是 `urlunparse()` 和 `urlunsplit()` 的反向操作，支持传入对应的 6 个条目数据或 5 个条目数据来将其组成标准的 URL 字符串，传入的数据必需是可迭代对象。

**特别注意，传入的数据必选遵循对应 URL 结构顺序，缺少的使用空白字符串占位。**

比如下述代码：

```python
import urllib.parse as urlparse  

# urlunparse() 传入 6 个参数，有分号路径参数
data1 = ('https', 'example.com', '/search',   
         'param=value', 'q=Windows+10&qs=MB', 'Win')  
url1 = urlparse.urlunparse(data1)  
print(url1)
# https://example.com/search;param=value?q=Windows+10&qs=MB#Win

# 传入参数如果不需要，必选使用空白字符串占位
data2 = ('https', 'example.com', '/search',  
         '', '', '')  
url2 = urlparse.urlunparse(data2)  
print(url2)
# https://example.com/search

# urlunsplit() 传入 5 个参数，没有分号路径参数
data3 = ('https', 'example.com', '/search',  
         'q=Windows+10&qs=MB', 'Win')  
url3 = urlparse.urlunsplit(data3)  
print(url3)
# https://example.com/search?q=Windows+10&qs=MB#Win
```

# 合并路由到 URL 中

两个路由的合并比参数的合并更为简单，使用 `urllib.parse.urljoin()` 依次传入基准 URL 路径和需要添加的 URL 路径，即可合并两者为一个新 URL 路径。

比如下述代码所示：

```python
import urllib.parse as urlparse  
  
url = urlparse.urljoin('https://example.com', 'index.html')  
print(url)
# https://example.com/index.html
```

# 拓展阅读：URL 转码

`urllib.parse()` 除了上述的分析合并功能外，还提供 URL 编码解码功能。支持使用 `quote()`，`quote_plus()`，`unquote()`，`unquote_plus()` 将 `%xx` 这类 URL 转义字符解码为可识别的字符，或将特殊字符或中文字符编码为 `%xx` 格式。

因为篇幅有限，在此不做详细介绍，更多请查看以下文章：

> [ 如何使用 Python 进行 URL 编解码 ](https://zhuanlan.zhihu.com/p/1941504564953064373)

————————————

> [ urllib.parse --- 将 URL 解析为组件 ](https://docs.python.org/zh-cn/3.13/library/urllib.parse.html)
