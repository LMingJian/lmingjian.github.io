---
title: Python telnetlib 的使用
date: 2021-01-15
author: LM
---

## 1.简介

Python 的一个远程 Telnet 模块，通过该模块可使用 Telnet 连接远程服务器。

## 2.使用

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
