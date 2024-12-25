---
title: 如何使用 Python 构造 WebKitFormBoundary 参数
date: 2024-12-25T16:09:02+08:00
author: LiangMingJian
---

# 需求

在请求 HTML 接口时，有时候需要上传文件，而接口通常需要使用 WebKitFormBoundary 格式参数来进行文件的上传。

# 代码实现

```python
# 构造函数
def WebKit_format(data, boundary="----WebKitFormBoundary*********ABC", headers=None):
    # 从headers中提取boundary信息
    if headers is None:
        headers = {}
    if "content-type" in headers:
        fd_val = str(headers["content-type"])
        if "boundary" in fd_val:
            fd_val = fd_val.split(";")[1].strip()
            boundary = fd_val.split("=")[1].strip()
        else:
            raise Exception("multipart/form-data头信息错误，请检查content-type key是否包含boundary")
    # form-data格式定式
    jion_str = '--{}\r\nContent-Disposition: form-data; name="{}"\r\n\r\n{}\r\n'
    end_str = "--{}--".format(boundary)
    args_str = ""
    if not isinstance(data, dict):
        raise Exception("multipart/form-data参数错误，data参数应为dict类型")
    for key, value in data.items():
        args_str = args_str + jion_str.format(boundary, key, value)
    args_str = args_str + end_str.format(boundary)
    args_str = args_str.replace("\'", "\"")
    return args_str

boundary_body = WebKit_format(data=bodyData, headers=headers)
"""
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="v"

2.0
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="req"

{"cno": "1213058673616305"}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="sig"

1
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="ts"

1
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="appid"

dp3wY4YtycajNEz23zZpb5Jl
------WebKitFormBoundary7MA4YWxkTrZu0gW--
"""
```
