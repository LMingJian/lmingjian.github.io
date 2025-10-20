---
title: Python Package：paramiko（ssh 连接工具）
date: 2025-10-13T10:51:47+08:00
author: LiangMingJian
---

# 需求

在某些时候，我们需要对安装在服务器的平台进行维护，比如执行些重启命令，执行些查询命令。这些操作我们正常都是通过 SSH 连接软件登录服务器然后输入对应指令执行的。每次需要执行这些操作都需要重复这些繁琐的登录，输入操作，那能不能通过 Python 将这些操作写成脚本，然后一键执行。

答案是肯定的，Python 提供了模块 paramiko 用于完成 SSH 远程连接和操作。

# 安装

```python
pip install paramiko
```

# SSH 连接

paramiko 模块通过实例化 `SSHClient()` 客户端来实现 SSH 连接，SSH 客户端通过 `connect()` 方法与远端服务器连接，在成功连接后，便可以通过 `exec_command()` 方法传输单条命令执行。

特别的，`exec_command()` 支持 timeout 参数，用来在命令执行超时时安全退出。此外 `exec_command()` 方法返回会返回 `stdin, stdout, stderr` 三个结果，分别表示输入，输出，错误结果，其中输出和错误结果支持通过 `read()` 方法读取后解码显示。

```python
import paramiko 

# 创建 SSH 客户端 
ssh = paramiko.SSHClient()  
# 设置连接到没有已知主机密钥的服务器时要使用的策略
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())  
# 连接客户端
ssh.connect(hostname="example.com", port=22, 
			username="user", password="password")  
# 执行指令
stdin, stdout, stderr = ssh.exec_command("ls -l", timeout=60)  
# 输出结果
print(stdout.read().decode())  
# 关闭连接
ssh.close()
```

特别的，因为 SSH 连接需要将服务器主机名与本地密钥中的主机名进行确认，只有存在于本地密钥的主机才能进行连接。所以第一次连接或没有加载本地密钥时，SSH 连接即使有密码，也会连接失败，并出现下述的报错提示：

```python
paramiko.ssh_exception.SSHException: Server '192.168.1.230' not found in known_hosts
```

此时，开发者可以通过策略方法 `set_missing_host_key_policy()` 在 SSH 服务器的主机名不在系统主机密钥，也不在应用程序密钥时进行对应的策略操作。

一般的策略支持：

- `paramiko.AutoAddPolicy()`：自动将主机名和密钥添加到本地对象，保存并使用
- `paramiko.RejectPolicy()`：自动拒绝未知主机和密钥
- `paramiko.WarningPolicy()`：记录以及警告未知主机和密钥，然后保存并使用

另外，如果开发者知道密钥所在位置，那便可以直接加载密钥，而不用设置策略来连接。

> 本地密钥往往储存在：`C:\Users\<用户名>\.ssh\`，`~/.ssh/`

```python
import paramiko 

ssh = paramiko.SSHClient()  

# 加载系统默认的 known_hosts 文件
ssh.load_system_host_keys()  
# 加载指定位置的 known_hosts 文件
ssh.load_host_keys('./custom_known_hosts')
# 连接服务器
ssh.connect(hostname="example.com", port=22, 
			username="user", password="password")  
# 对本次连接的地址进行存储
ssh.save_host_keys('./custom_known_hosts')
# 执行命令
stdin, stdout, stderr = ssh.exec_command("ls -l")  
print(stdout.read().decode())  
ssh.close()
```

最后，如果存在私钥文件，可以直接使用私钥文件进行登录，而不用输入密码。

```python
import paramiko 

ssh = paramiko.SSHClient() ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())

# 加载私钥
private_key = paramiko.RSAKey.from_private_key_file('./private_key') 
# 建立连接
ssh.connect(hostname='example.com', port=22, 
			username='user', pkey=private_key) 
# 另外也可以直接使用私钥文件
ssh.connect(hostname='example.com', port=22, 
			username='user', key_filename='./private_key') 

stdin, stdout, stderr = ssh.exec_command("ls -l")  
print(stdout.read().decode())  
ssh.close()
```

# SSH 连续命令输入

上面代码中的 `exec_command()` 方法适用于**执行单个命令，一次调用执行一次命令，多次调用间的环境不互通**。

比如执行 `cd /home` 和 `ls -l` 这两个命令，如果使用 `exec_command()` 分别执行，结果不会输出 home 目录下的文件结构，因为每次执行的环境都是独立的。

要实现 home 目录下的文件输出，要不通过 `cd /home && ls -l` 将两个命令在一次传输中调用，要不就通过方法 `invoke_shell()` 创建一个用于交互式会话终端执行。

`invoke_shell()` 用于在服务器上创建一个交互式会话终端，并创建一个 SSH Shell Channel 通道来进行连接。用户可以通过该通道与远程服务器进行双向通信，完成一些复杂指令的执行。

```python
import time  
import paramiko  
  
ssh = paramiko.SSHClient()  
ssh.load_system_host_keys()  
ssh.connect(hostname="example.com", port=22, 
			username="user", password="password")  

# 创建会话终端和通道
channel = ssh.invoke_shell()  
# 发送指令
channel.send('cd /home\n'.encode())  
# 提供等待时间，确保命令执行完成  
time.sleep(1)  
channel.send('ls -l\n'.encode())  
time.sleep(1)  
# 检查通道数据是否准备好，然后全部读取
while channel.recv_ready():  
    output = channel.recv(4096).decode()  
    print(output.strip())  

ssh.close()
```

# 文件传输

paramiko 除了提供 SSH 连接外，还支持建立 SFTP 连接来传输文件。

```python
import paramiko  

# 构建 SFTP Transport 传输对象
transport = paramiko.Transport(('example.com', 22))  
# 连接服务器
transport.connect(username='user', password='password')  
# 开启传输客户端
sftp = transport.open_sftp_client()  
# 上传文件
sftp.put('./rtsp.txt', '/home/rtsp.txt')
# 下载文件
sftp.get('/home/example.txt', './example.txt')
# 关闭连接
sftp.close()  
transport.close()
```

————————————

> [ Paramiko 文档 ](https://docs.paramiko.org/en/stable/index.html)
