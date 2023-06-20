---
title: Python 实现 WebKitFormBoundary 参数构造
date: 2023-03-18
author: LM
---

## 1.需求

在请求 HTML 接口时，有时候需要上传文件，而接口通常需要使用 WebKitFormBoundary 格式参数来进行文件的上传。

## 2.实现

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

# 调用
def test001_auth_login():
    headers = {'Accept': "application/json",
               'Content-Type': "multipart/form-data; boundary=----WebKitFormBoundary*********ABC",
               'Accept-Encoding': "gzip, deflate, br", 'Connection': "keep-alive"}
    url = "https://www.baidu.com/login/"
    bodyData = {
        "username": 1234567890,
        "password": "a123456",
        "type": "account",
        "bindType": "null",
        "openId": "null"
    }
    boundary_body = WebKit_format(data=bodyData, headers=headers)
    print(boundary_body)
    response = requests.post(url, data=boundary_body, headers=headers)
    access_token = regSearchString(response.text, '"access_token":"(.+?)",', 16, -2)
    print("正则--获取登陆的access_token:  " + access_token)
    print(response.text)
    time.sleep(1)
    
# 输出
boundary_body = WebKit_format(data=bodyData, headers=headers)

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
```

