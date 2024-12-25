---
title: Python Package：telnetlib
date: 2024-12-25T16:12:53+08:00
author: LiangMingJian
---

# 概述

Python 的一个远程 Telnet 模块，通过该模块可使用 Telnet 连接远程服务器。

# 使用

```python
import getpass
import telnetlib

HOST = "localhost"
user = input("Enter your remote account: ")
password = getpass.getpass()

tn = telnetlib.Telnet(HOST)

tn.read_until(b"login: ")
tn.write(user.encode('ascii') + b"\n")
if password:
    tn.read_until(b"Password: ")
    tn.write(password.encode('ascii') + b"\n")

tn.write(b"ls\n")
tn.write(b"exit\n")

print(tn.read_all().decode('ascii'))
```
