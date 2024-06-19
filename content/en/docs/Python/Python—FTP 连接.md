---
title: FTP 的连接
date: 2024-06-19
author: LiangMingJian
---

## 1.使用 ftplib 连接

```python
import os
from ftplib import FTP

def ftp_down(filename = "xx.tar.gz"):
    ftp=FTP()
    ftp.set_debuglevel(2)  # 设置调试等级
    ftp.connect('127.0.0.1','21')
    ftp.login('user','passwd')
    ftp.set_pasv(False)       # False：主动模式   True：被动模式
    print ftp.getwelcome()    # 显示 ftp 服务器的欢迎信息 
    ftp.cwd('/')     # 选择操作目录 
    bufsize = 1024
    filename_local = "xx.tar.gz"
    file_handler = open(filename_local,'wb').write  # 以写模式在本地打开文件 
    ftp.retrbinary('RETR %s' % os.path.basename(filename),file_handler,bufsize)    # 接收服务器上文件并写入本地文件 
    ftp.set_debuglevel(0)
    ftp.quit()
    print "ftp down OK"

ftp_down()
```

## 2.EX.FTP 的主动模式与被动模式

在主动模式下，FTP 客户端会随机开启一个大于 1024 的端口 N 向服务器的 21 号端口发起连接，并发送 FTP 用户名和密码，然后开放 N+1 号端口进行监听，并向服务器发出 PORT N+1命令，告诉服务端客户端采用主动模式并开放了端口。FTP 服务器接收到 PORT 命令后，会用其本地的 FTP 数据端口（通常是20）来连接客户端指定的端口 N+1，进行数据传输。

**在主动模式中，服务器主动建立数据连接到客户端的随机端口。**

在被动模式下，FTP 客户端随机开启一个大于 1024 的端口 N 向服务器的 21 号端口发起连接，发送用户名和密码进行登陆，同时会开启 N+1 端口。然后向服务器发送 PASV 命令，通知服务器自己处于被动模式。服务器收到命令后，会开放一个大于 1024 的端口 P（端口 P 的范围可以设置）进行监听，然后用 PORT P 命令通知客户端，自己的数据端口是 P。客户端收到命令后，会通过 N+1 号端口连接服务器的端口 P，然后在两个端口之间进行数据传输。

**在被动模式中，客户端主动建立数据连接到服务器的随机端口。**

一般情况下，用户可以在对应的 FTP 客户端软件中配置连接方式为主动还是被动，如 WinSCP，设置位于编辑 > 高级 > 连接 > 被动模式。