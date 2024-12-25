---
title: 如何使用 Python requests 流式传输数据
date: 2024-12-25T16:10:23+08:00
author: LiangMingJian
---

# 概述

在使用 Python requests 进行网络请求时，有时候会进行数据下载或上传，当数据量过大时，容易出现内存溢出的问题。此时，就需要使用流式传输。

# 大文件流式读取

通过设置形参 stream 来让请求以流的形式发送，而不用将响应全部写入内存。

- 当 stream=True 时，响应不会立即读取到内存中，用户可以使用 Chunk 分段读取。
- 当 stream=False 时，响应会立即读取到内存中。

```python
response = requests.get(url, headers=headers, stream=True)
with open(file_name, mode='wb') as file: 
    for chunk in response.iter_content(chunk_size=chunk_size):
        file.write(chunk)
```
