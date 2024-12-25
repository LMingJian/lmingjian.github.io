---
title: Python Package：MultipartEncoder
date: 2024-12-25T16:11:35+08:00
author: LiangMingJian
---

# 概述

MultipartEncoder 是使用于上传文件的一个模块，其包含在模块 requests_toolbelt 中，通过安装 requests_toolbelt 来使用该模块。

```python
pip install requests_toolbelt
```

# 示例

```python
from requests_toolbelt import MultipartEncoder

encoder = MultipartEncoder({
            'field': ('file_name', b'{"a": "b"}', 'application/json',
                      {'X-My-Header': 'my-value'})
        })
# field：服务端约定的上传文件字段名。一般用到的是file，需要和服务端沟通获取
# file_name: 文件名。一般可以任意写，服务端大多是拿到文件后自己再次命名
# b'{"a":"b"}'：文件内容，以二进制代码存在。例：open('/your/file/path', 'rb')
# 'application/json'：文件的MimeType。不同文件类型需要对应不同的 MimeType
# {'X-My-Header': 'my-value'}：其他内容，可不传。
payload = {
    'file': ('upload.pdf', open('sync_test.pdf', 'rb'), 'application/pdf')
}
m = MultipartEncoder(payload)
```

参照官方给予的示例代码，可以仿照仿照出下列代码：

```python
import requests
from requests_toolbelt import MultipartEncoder


upload_url = 'https://your/upload/url'
payload = {
    'file': ('upload.pdf', open('sync_test.pdf', 'rb'), 'application/pdf')
}
m = MultipartEncoder(payload)
headers = {
    "Content-Type": m.content_type,
    "other-keys": "other-values"
}
r = requests.post(upload_url, headers=headers, data=m)
print(r.json())
```

{{< details "参考文件" >}} 
1：[ 上传文件模块 MultipartEncoder @山猪打不过家猪 ](https://www.jianshu.com/p/9738e53a7db3)
{{< /details >}}
