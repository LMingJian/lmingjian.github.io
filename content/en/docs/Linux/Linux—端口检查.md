---
title: Linux 检查端口占用情况
date: 2022-01-20
author: LM
---

## 1.使用 lsof 检查

`lsof`是一个列出当前系统打开文件的工具，可以使用它来查看端口占用的情况。

```bash
lsof -i:端口号
```

## 2.使用 netstat 检查

`netstat -tunlp` 可以用于展示 tcp，udp 端口和进程的相关情况。

```bash
netstat -tunlp | grep 端口号
```

- -t (tcp) 仅显示 tcp 相关选项
- -u (udp)仅显示 udp 相关选项
- -n 拒绝显示别名，能显示数字的全部转化为数字
- -l 仅列出在 Listen (监听)的服务状态
- -p 显示建立相关链接的程序名

