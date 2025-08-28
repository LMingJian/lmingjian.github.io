---
title: 如何使用 Python 进行邮件发送
date: 2024-12-25T16:09:28+08:00
author: LiangMingJian
---

# 需求

通过 Python 代理邮箱发送邮件。

# 代码实现

```python
import smtplib
import time
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.application import MIMEApplication
import os

def send_email(sender:str, password:str, receivers:list[str],
               subject:str, content:str, subtype:str='html', 
               attachments:list[str]=None ,
               smtp_server:str='smtp.qq.com', port:int=465):
    """
    SMTP 邮件发送

    :param sender: 发件邮箱
    :param password: 授权码
    :param receivers: 收件人列表，支持群发
    :param subject: 邮件主题
    :param content: 邮件内容
    :param subtype: 内容类型 html/plain
    :param attachments: 附件路径列表
    :param smtp_server: SMTP 服务器
    :param port: 端口号 SSL 用 465，TLS 用 587
    """
    msg = MIMEMultipart()
    msg['From'] = sender
    msg['To'] = ', '.join(receivers)
    msg['Subject'] = subject

    # 添加正文
    msg.attach(MIMEText(content, subtype, 'utf-8'))

    # 添加附件
    if attachments:
        for file in attachments:
            with open(file, 'rb') as f:
                part = MIMEApplication(f.read())
                part.add_header('Content-Disposition', 'attachment',
                                filename=os.path.basename(file))
                msg.attach(part)

    # SSL 加密连接
    server = smtplib.SMTP_SSL(smtp_server, port)
    server.set_debuglevel(0)  # 1/0: 开启/关闭调试
    server.ehlo(smtp_server)  # 声明客户端域名
    server.login(sender, password)  # 登录 SMTP 服务器

    # 发送邮件
    server.sendmail(sender, receivers, msg.as_string())

    # 关闭连接
    server.quit()

if __name__ == '__main__':
    # 使用
    send_email(
        sender='******@qq.com',
        password='******',
        receivers=['******@qq.com', '******@qq.com', '******@qq.com'],
        subject='Python 邮件测试',
        content=f'<h1>Python 邮件测试</h1><p>正文</p><p>{time.time()}</p>',
        attachments=['******.txt', '******.txt']
    )
```