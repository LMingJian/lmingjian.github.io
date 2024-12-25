---
title: Python Package：requests
date: 2024-12-25T16:12:47+08:00
author: LiangMingJian
---

# 概述

requests 是一个基于 urllib 编写的 HTTP 库，相比 urllib 库，requests 库更加方便，能轻易的实现各种 HTTP 测试需求。requests 库经常被用来进行接口测试以及爬虫。

```python
pip install requests
```

# 使用

```python
response = requests.get(url)
```

# 支持的参数

```
| 参数            | 解释                                                     
| --------------- | ------------
| method          | 请求方法，比如 get、options、head、post、put、patch、delete
| url             | 请求的 url  
| params          | 请求携带的 params     
| data            | 请求 body 中的 data  
| json            | 请求 body 中的 json 格式的 data  
| headers         | 请求携带的 headers
| cookies         | 请求携带的 cookies   
| files           | 上传文件时使用
| auth            | 身份认证时使用     
| timeout         | 设置请求的超时时间，可以设置连接超时和读取超时               
| allow_redirects | 是否允许重定向，默认 True，即允许重定向                       
| proxies         | 设置请求的代理，支持 http 代理以及 socks 代理
| verify          | 用于 https 请求时的 ssl 证书验证，默认是开启的，如果不需要则设置为 False 即可
| stream          | 是否立即下载响应内容，默认是 False，即立即下载响应内容       
| cert            | 用于指定本地文件用作客户端证书                           
```

# 支持的对象

```
| 属性或属性方法           | 解释                                                     
| ----------------------- | ------
| r.status_code           | 响应的 http 状态码，比如 404 和 200  
| r.json()                | 将响应解析成 json 格式    
| r.headers               | 响应头
| r.raw                   | 原始响应
| r.url                   | 请求的最终地址 
| r.encoding              | 要解码的 r.text 的编码方式   
| r.history               | 请求的历史记录，可以用于查看重定向信息，以列表形式展示，排序方式是从最旧到最新
| r.reason                | 响应状态的描述，比如 "Not Found" or "OK" 
| r.cookies               | 服务器发回的cookies，RequestsCookieJar类型  
| r.elapsed               | 从发送请求到响应到达之间经过的时间，用于测试响应速度   
| r.request               | 用于查看发送请求时的信息
| r.ok                    | 检查 status_code 的值，如果小于 400，则返回 True，如果不小于 400，则返回 False 
| r.is_redirect           | 判断是否重定向，返回 True or False      
| r.is_permanent_redirect | 判断是否永久重定向，返回 True or False   
| r.next                  | 返回重定向链中下一个请求的 PreparedRequest 对象
| r.apparent_encoding     | 用 chardet 库判断出的编码方式  
| r.content               | 响应的内容，byte 类型   
| r.text                  | 响应的内容，unicode 类型  
| r.links                 | 响应的解析头链接  
```
