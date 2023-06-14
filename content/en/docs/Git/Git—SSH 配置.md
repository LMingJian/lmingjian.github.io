---
title: Git 的 SSH 钥匙配对
date: 2021-01-06
author: LM
---

## 1.配置 SSH

打开 Git 命令行工具，输入以下命令，使用邮箱创建密码对。


```bash
ssh-keygen -t rsa -b 2048 -C "email@example.com"
```

有此结果，在对应目录`/home/user/.ssh/id_rsa`下找到生成的公钥文件 id_rsa.pub，记事本打开，将里面的内容复制到剪贴板。

```bash
Generating docs/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa):
```

打开新建的 github 或者 gitlab 账户，找到 SSH Keys 选项`setting->SSH keys`，将剪贴板内容粘贴到内容框中，title 可以用默认的邮箱名字，最后点击 add。这就代表这个用户被远程仓库所承认了。

选择克隆的项目，复制 SSH 克隆 URL，进行克隆。

```bash
git clone + 库的地址
```

## 2.创建 SSH KEY 的目的

SSH KEY 的作用是来识别你的电脑，相当于人的身份证号，在你的 C 盘用户目录下存在一个 .ssh 目录，这个目录下有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的秘钥对，id_rsa 是私钥，不能泄露出去，id_rsa.pub 是公钥，可以放心地告诉任何人。