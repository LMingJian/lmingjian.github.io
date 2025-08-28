---
title: 如何使用 Python 连接 FTP 服务器
date: 2024-12-25T16:10:09+08:00
author: LiangMingJian
---

# 前言

在 Python 中，可以通过 Python 标准库中的 ftplib 来实现对 FTP 服务器的操作。

# 连接与登录

```python
from ftplib import FTP

# 实例化 FTP
ftp=FTP()

# 设置调试等级, 0 不调试, 1 只打印命令与响应, 2 打印详细信息
ftp.set_debuglevel(0)

# 设置 FTP 主动被动模式, False 主动模式, True 被动模式(默认的模式)
ftp.set_pasv(True)

# 设置编码格式
ftp.encoding = 'utf-8'

# 连接与登录
ftp.connect('******',21)
ftp.login('******','******')

# 打印欢迎信息
print(ftp.getwelcome())

# 退出客户端
ftp.quit()
```

# 列出目录信息

```python
...

# 查看当前所在目录
print(ftp.pwd())

# 列出当前目录的文件与子目录
ftp.dir()

# 将结果保存到列表中
date1 = []
ftp.dir(date1.append)
print(date1)

# 仅获取文件或目录名称，并存放为列表
date2 = ftp.nlst()
print(date2)

...
```

# 目录切换与管理

```python
...

# 切换目录
print(ftp.pwd())
ftp.cwd('CH1')
print(ftp.pwd())

# 新建目录
ftp.mkd('Test')

# 删除目录
ftp.rmd('Test')

...
```

# 上传文件

```python
...

# 基本使用, 注意文件以二进制形式读取
# blocksize 每次传输的数据大小，默认 8192 = 8KB
file_name = 'requirements.txt'
with open('requirements.txt', 'rb') as f:
    ftp.storbinary(f'STOR {file_name}', f, blocksize=1024)

# 显示上传进度
def progress(data):
    global transferred
    transferred += len(data)
    print(f"已上传: {transferred} bytes")

transferred = 0
file_name = 'share.mp4'
with open('share.mp4', 'rb') as f:
    ftp.storbinary(f'STOR {file_name}', f, blocksize=1024, callback=progress)

...
```

# 下载文件

```python
...

# 基本使用, 注意文件以二进制形式读取
# blocksize 每次传输的数据大小，默认 8192 = 8KB
file_name = 'requirements.txt'
with open('requirements.txt', 'wb') as f:
    ftp.retrbinary(f'RETR {file_name}', f.write, blocksize=1024)

# 显示下载进度
def progress(data, file):
    global transferred
    file.write(data)
    transferred += len(data)
    print(f"已下载: {transferred} bytes")

transferred = 0
file_name = 'share.mp4'
with open('share.mp4', 'wb') as f:
    ftp.retrbinary(f'RETR {file_name}', 
                   lambda date: progress(date, f), 
                   blocksize=1024)

...
```

# 删除文件

```python
...

# 指定当前目录下文件名
ftp.delete('share.mp4')

...
```

# 拓展阅读

**FTP 的主动模式与被动模式**

**在主动模式中，服务器主动建立数据连接到客户端的随机端口。**

此时，FTP 客户端会随机开启一个大于 1024 的端口 N 向服务器的 21 号端口发起连接，并发送 FTP 用户名和密码，随后开放 N+1 号端口进行监听，并向服务器发出 PORT N+1 命令，告诉服务端，客户端采用主动模式并开放了端口。

FTP 服务器在接收到 PORT 命令后，会用其本地的 FTP 数据端口（通常是20）来连接客户端指定的端口 N+1，进行数据传输。

**在被动模式中，客户端主动建立数据连接到服务器的随机端口。**

此时，FTP 客户端随机开启一个大于 1024 的端口 N 向服务器的 21 号端口发起连接，并发送用户名和密码进行登陆，同时开启 N+1 端口，向服务器发送 PASV 命令，通知服务器自己处于被动模式。

服务器收到命令后，会开放一个大于 1024 的端口 P（端口 P 的范围可以设置）进行监听，然后用 PORT P 命令通知客户端，自己的数据端口是 P。客户端收到命令后，会通过 N+1 号端口连接服务器的端口 P，然后在两个端口之间进行数据传输。

一般情况下，用户可以在对应的 FTP 客户端软件中配置连接方式为主动还是被动，如 WinSCP，设置位于：编辑 > 高级 > 连接 > 被动模式。
