---
title: Python 模块：requests
date: 2020-09-18
author: LM
---

## 1.简介

Requests 是一个基于 urllib 编写的 HTTP 库，相比 urllib 库，Requests 库更加方便，能轻易的实现各种 HTTP 测试需求。Requests 库经常被用来进行接口测试以及爬虫。

```
pip install request
```

## 2.使用

```
response = requests.get(url)
```

## 3.大文件流式读取

通过设置形参 stream 来让请求以流的形式发送，而不用将响应全部写入内存，造成内存溢出。当 stream=True 时，响应不会立即读取到内存中，用户可以使用 Chunk 分段读取，当 stream=False 时，响应会立即读取到内存中。

```python
response = requests.get(url, headers=headers, stream=True)
with open(file_name, mode='wb') as file: 
    for chunk in response.iter_content(chunk_size=chunk_size):
        file.write(chunk)
```

## 4.参数

```
| 参数            | 解释                                                     
| --------------- | ------------------------------------------------------------ 
| method          | 请求方法，比如get、options、head、post、put、patch、delete
| url             | 请求的url                                                    
| params          | 请求携带的params                                             
| data            | 请求body中的data                                             
| json            | 请求body中的json格式的data                                   
| headers         | 请求携带的headers                                            
| cookies         | 请求携带的cookies                                            
| files           | 上传文件时使用                                               
| auth            | 身份认证时使用                                               
| timeout         | 设置请求的超时时间，可以设置连接超时和读取超时               
| allow_redirects | 是否允许重定向，默认True，即允许重定向                       
| proxies         | 设置请求的代理，支持http代理以及socks代理（需要安装第三方库"pip install requests[socks]"） 
| verify          | 用于https请求时的ssl证书验证，默认是开启的，如果不需要则设置为False即可 
| stream          | 是否立即下载响应内容，默认是False，即立即下载响应内容       
| cert            | 用于指定本地文件用作客户端证书                           
```

## 5.对象

```
| 属性或属性方法           | 解释                                                     
| ----------------------- | ------------------------------------------------------------ 
| r.status_code           | 响应的http状态码，比如404和200                               
| r.json()                | 将响应解析成json格式                                         
| r.headers               | 响应头，可单独取出某个字段的值，比如(r.headers)['content-type'] 
| r.raw                   | 原始响应，表示urllib3.response.HTTPResponse对象。使用raw时，要求在请求时设置“stream=True” 
| r.url                   | 请求的最终地址                                               
| r.encoding              | 要解码的r.text的编码方式                                     
| r.history               | 请求的历史记录，可以用于查看重定向信息，以列表形式展示，排序方式是从最旧到最新 
| r.reason                | 响应状态的描述，比如 "Not Found" or "OK"                     
| r.cookies               | 服务器发回的cookies，RequestsCookieJar类型                   
| r.elapsed               | 从发送请求到响应到达之间经过的时间量，可以用于测试响应速度   
| r.request               | 可以用于查看发送请求时的信息，比如r.request.headers查看请求头 
| r.ok                    | 检查”status_code“的值，如果小于400，则返回True，如果不小于400，则返回False 
| r.is_redirect           | 判断是否重定向，返回True or False                            
| r.is_permanent_redirect | 判断是否永久重定向，返回True or False                        
| r.next                  | 返回重定向链中下一个请求的PreparedRequest对象               
| r.apparent_encoding     | 用chardet库判断出的编码方式                                  
| r.content               | 响应的内容，byte类型                                        
| r.text                  | 响应的内容，unicode类型                                
| r.links                 | 响应的解析头链接                                             
```



