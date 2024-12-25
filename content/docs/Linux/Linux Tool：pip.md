---
title: Linux Tool：pip
date: 2024-12-25T13:59:08+08:00
author: LiangMingJian
---

# 概述

不同于 python3 自带 pip 软件管理工具，Linux 默认安装的 python2 并不自带，需要自己安装。

```bash
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
# pip3 wget https://bootstrap.pypa.io/get-pip.py
# wget下载命令，会下载到当前目录，如报错，yum -y install wget
python get-pip.py #编译
pip  -V
```

*PS：如果不想自己安装，可以使用`yum install python3`安装 python3 来使用 pip，命令 `pip3 -V`*

# 修改 pip 下载源

```bash
pip -i https://pypi.douban.com/simple install Flask --trusted-host pypi.douban.com #手动指定
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ #环境指定
###############
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：https://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
华中理工大学：https://pypi.hustunique.com/
山东理工大学：https://pypi.sdutlinux.org/
豆瓣：https://pypi.douban.com/simple/
```

*PS：如果系统报错`ERROR: unknown command "config"`，这是因为 pip 版本过低，使用命令 `pip install -U pip`即可*

# pip 的使用

```bash
pip list
pip list --outdated  # 列出可更新的 package
pip install --upgrade package  # 更新 package
pip install package  # 安装 package
```
