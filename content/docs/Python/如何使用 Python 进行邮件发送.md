---
title: 如何使用 Python 进行邮件发送
date: 2024-12-25T16:09:28+08:00
author: LiangMingJian
---

# 需求

通过 Python 代理邮箱发送邮件。

# 示例：基本的邮件发送

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.image import MIMEImage
 
# qq 邮箱的 smtp 服务器
host_server = 'smtp.qq.com'
# 设置发件人的 qq 号码
sender_qq = '********@qq.com'
# 设置 qq 邮箱的授权码
pwd = '********'
# 设置发件人的邮箱
sender_qq_mail = '********@qq.com'
# 设置收件人邮箱
receiver = '********@qq.com'

# 编写邮件的正文内容
mail_content = '你好，这是邮件'
# 邮件标题
mail_title = 'Max\'s的邮件'

# ssl 登录
smtp = SMTP_SSL(host_server)
# set_debuglevel() 调试，1 表示开启调试模式，0 关闭调试模式
smtp.set_debuglevel(0)
smtp.ehlo(host_server)
smtp.login(sender_qq, pwd)

msg = MIMEText(mail_content, "plain", 'utf-8')
msg["Subject"] = Header(mail_title, 'utf-8')
msg["From"] = sender_qq_mail
msg["To"] = receiver
smtp.sendmail(sender_qq_mail, receiver, msg.as_string())
smtp.quit()
```

# 示例：发送带附件的邮件

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.image import MIMEImage
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication 
 
if __name__ == '__main__':
        fromaddr = '********@163.com'
        password = '********'
        toaddrs = ['********@163.com', '********@qq.com']
 
        content = 'hello, this is email content.'
        textApart = MIMEText(content)
 
        imageFile = '1.png'
        imageApart = MIMEImage(open(imageFile, 'rb').read(), imageFile.split('.')[-1])
        imageApart.add_header('Content-Disposition', 'attachment', filename=imageFile)
 
        pdfFile = 'PDF1.pdf'
        pdfApart = MIMEApplication(open(pdfFile, 'rb').read())
        # 使用 MIMEApplication 添加附件
        pdfApart.add_header('Content-Disposition', 'attachment', filename=pdfFile)
    
        zipFile = 'ZIP1.zip'
        zipApart = MIMEApplication(open(zipFile, 'rb').read())
        zipApart.add_header('Content-Disposition', 'attachment', filename=zipFile)
 
        m = MIMEMultipart()
        m.attach(textApart)
        m.attach(imageApart)
        m.attach(pdfApart)
        m.attach(zipApart)
        m['Subject'] = 'title'
 
        try:
            server = smtplib.SMTP('smtp.163.com')
            server.login(fromaddr,password)
            server.sendmail(fromaddr, toaddrs, m.as_string())
            print('success')
            server.quit()
        except smtplib.SMTPException as e:
            print('error:',e)
```
