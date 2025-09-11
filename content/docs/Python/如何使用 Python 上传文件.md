---
title: 如何使用 Python 上传文件
date: 2024-12-25T16:09:02+08:00
author: LiangMingJian
---

# 需求

在请求接口时，往往会遇到上传文件的需求，此时如何通过 Python 构造请求参数，并成功调用上传接口就成了一个问题。

在 HTTP 上传文件时，通常使用媒体类型 `multipart/form-data` 来作为请求数据格式来进行传递。`multipart/form-data` 的数据体由多个部分组成，这些部分由一个固定边界值Boundary 分隔，这个边界值往往在浏览器中写成如下格式：

```
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="v"

2.0
```

显然，通过构造一个类似上述格式的请求参数，即可成功调用上传接口。

# 使用 `requests.post(url, files=files)`

注意，虽然 `requests` 能自动生成 `form-data` 格式请求参数。但在实际传输转换时，其 `WebKitFormBoundary` 会是随机字符串，如果服务器端带有验证可能会出现问题。

```python
import requests

data = {'name': 'requirements.txt', 'type': 'txt'}
files = {'file': open('requirements.txt', 'rb')}
response = requests.Request('POST', 'http://example.com', data=data, files=files)
prepped = response.prepare()
print(prepped.body.decode('utf-8'))
```

输出结果：

```
--6f9d938b58012cf84abc920695246ee4
Content-Disposition: form-data; name="name"

requirements.txt
--6f9d938b58012cf84abc920695246ee4
Content-Disposition: form-data; name="type"

txt
--6f9d938b58012cf84abc920695246ee4
Content-Disposition: form-data; name="file"; filename="requirements.txt"

**文件内容**
--6f9d938b58012cf84abc920695246ee4--
```

# 使用 requests_toolbelt

实际使用时，建议考虑 `requests_toolbelt` 中的 `MultipartEncoder`，其能使用简单的方法生成符合格式的请求参数。

```python
from requests_toolbelt import MultipartEncoder

data = MultipartEncoder(
    fields={
        'name': 'requirements.txt',
        'type': 'txt',
        'file': ('requirements.txt', open('requirements.txt', 'rb')),
    },
    boundary='----WebKitFormBoundary0123456789'
)
print(data.to_string().decode('utf-8'))
```

输出结果：

```
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="name"

requirements.txt
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="type"

txt
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="file"; filename="requirements.txt"

**文件内容**
------WebKitFormBoundary0123456789--
```

# 自定义构造

```python
def WebKitFormBoundary(data: dict, boundary="----WebKitFormBoundary0123456789"):   
    join_str = '--{}\r\nContent-Disposition: form-data; name="{}"\r\n\r\n{}\r\n'  
    end_str = "--{}--".format(boundary)  
    args_str = ""  
    for key, value in data.items():  
        args_str = args_str + join_str.format(boundary, key, value)  
    args_str = args_str + end_str.format(boundary)  
    args_str = args_str.replace("\'", "\"")  
    return args_str  
  
raw_data = {  
    'name': 'requirements.txt',  
    'type': 'txt',
    'file': open('requirements.txt', 'rb')  
}
  
form_data = WebKitFormBoundary(raw_data)  
print(form_data)
```

输出结果：

```
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="name"

requirements.txt
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="type"

txt
------WebKitFormBoundary0123456789
Content-Disposition: form-data; name="file"

<_io.BufferedReader name="requirements.txt">
------WebKitFormBoundary0123456789--
```
