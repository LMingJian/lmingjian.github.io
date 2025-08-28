---
title: 如何使用 Python requests 流式下载大文件
date: 2024-12-25T16:10:23+08:00
author: LiangMingJian
---

# 前言

在使用 Python requests 进行网络请求时，有时候会进行数据下载，当数据量过大时，如果直接将数据写入内存，则容易出现内存溢出的问题，此时，就需要使用流式传输。

# 大文件流式下载

通过设置形参 stream 来让请求以流的形式下载，而不用将响应全部写入内存。

- 当 stream=True 时，响应不会立即读取到内存中，用户可以使用 Chunk 分段读取。
- 当 stream=False 时，响应会立即读取到内存中。

在读取到响应后，使用 `response.iter_content(chunk_size)` 来分段写入数据，该方法的 chunk_size 以字节 B 为单位，当设置为 None 时，则按服务器返回数据大小写入。

```python
import requests  
  
url = 'http://example.com/bigfile.mp4'  
response = requests.get(url, stream=True)  
progress = 0
with open('bigfile.mp4', mode='wb') as file:  
    for chunk in response.iter_content(chunk_size=2048*1024):  
        file.write(chunk)  
        progress = progress + len(chunk)  
        print(f'当前已下载： {progress} B')
```
