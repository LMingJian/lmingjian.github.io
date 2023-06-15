---
title: Linux 中 Python3.6 安装
date: 2020-11-22
author: LM
---

## 1.前言

Linux 系统自带 Python2，这是由于部分命令需要使用 Python2，如 yum，要使用 Python3 则需另外安装。

（本文使用的是 CentOS7 ，其他发行版大同小异）

```bash
python -V #查看Py版本
python -m XXXX  #执行终端命令，-m参数使得Python预先import你要的package或module给你，然后再执行script。
which python #查看python命令是连接到那个文件
####
/usr/bin/python
####
cd /usr/bin/ #进入bin文件夹
ll python*  #列出包含python字段的文件
####
lrwxrwxrwx. 1 root root   16 11月 17 11:42 python -> /usr/bin/python2
lrwxrwxrwx. 1 root root    9 11月 13 11:01 python2 -> python2.7
-rwxr-xr-x. 1 root root 7144 10月 14 22:46 python2.7
-rwxr-xr-x. 1 root root 1835 10月 14 22:45 python2.7-config
lrwxrwxrwx. 1 root root   16 11月 16 17:40 python2-config -> python2.7-config
lrwxrwxrwx. 1 root root   14 11月 16 17:40 python-config -> python2-config
####
# 可以看出python指向python2，最终指向python2.7
```

## 2.编译安装 Python

```bash
yum -y install zlib zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel libffi-devel gcc-c++
# 安装软件依赖以编译文件（可选）
yum -y install gcc-c++ openssl-devel libffi-devel bzip2-devel
# 安装软件依赖以编译文件（必须有）
# 下载 Python 源码，解压，然后进入文件夹，编译到对应目录，安装
wget http://npm.taobao.org/mirrors/python/3.9.0/Python-3.9.0.tgz
tar -zxvf Python-3.9.0.tgz
cd Python-3.9.0 || { echo "Failure"; exit 1; }
./configure --prefix=/usr/local/python3
make && make install
# 编译安装后，使用 ln 进行软链接
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
# 配置 pip 源
pip3 config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 使用
python3 -V
pip3 -V
```

## 3.其他

```bash
# 若要将 python 命令指向 python3，可以先删除原链接再重新指向。但这里不推荐这样做，因为 Linux 的部分包需要使用 python2
rm /usr/bin/python
ln -s python3 /usr/bin/python
# 如需切换回 python2
ln -s python2 /usr/bin/python
# pip
cd /usr/bin
mv pip pip.bak #备份
ln -s pip3 /usr/bin/pip
```

