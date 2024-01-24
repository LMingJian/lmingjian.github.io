---
title: 修复 Python Request 模块 Url 编码异常的问题
date: 2021-09-16
author: LMingJian
---

## BUG 描述

Python 中用户使用 Requests 库发送 Http 请求时，请求的所有参数都会被进行 Url 编码。此时容易出现由于 Url 编码后参数异常的情况，特别是**中文字符**，最终导致 Http 请求失败。

## Resolution

用户可以将参数提前进行编码传递，以避免 Requests 库对参数的编码。

```python
payload1 = '{ABC}'
# String
data = payload1.encode('utf-8')
# b'{ABC}' 转换后的 UTF-8 编码
response = requests.request("POST", url, data=data)
```

## 复现

```python
data = {"name": "越秀第二中学", "is_famous": 2, "school_number": 440104, "principal": "", "telephone": "", "address": "", "email": "", "logo_url": "", "icon_url": "", "official_website": "", "org_web_path": 440104002, "area_id": 440103, "user_name": 440104002, "account": 440104002, "password": "7c4a8d09ca3762af61e59520943dc26494f8941b"}

# 被异常编码后数据
data = "name=%E8%B6%8A%E7%A7%80%E7%AC%AC%E4%BA%8C%E4%B8%AD%E5%AD%A6&is_famous=2&school_number=440104&principal=&telephone=&address=&email=&logo_url=&icon_url=&official_website=&org_web_path=440104002&area_id=3&user_name=440104002&account=440104002&password=7c4a8d09ca3762af61e59520943dc26494f8941b"
```

