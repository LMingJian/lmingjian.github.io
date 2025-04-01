---
title: Linux Tool：Python3.6
date: 2024-12-25T13:59:12+08:00
author: LiangMingJian
---

# 概述

Linux 系统自带 Python2，这是由于部分命令需要使用 Python2，如 yum，要使用 Python3 则需另外安装。

（本文使用的是 CentOS7 ，其他发行版大同小异）

```shell
python -V # 查看 Python 版本
python -m XXXX  # 执行终端命令，-m 参数使得 Python 预先 import 你要的 package 或module 给你，然后再执行 script。
which python # 查看 python 命令是连接到那个文件
>>> /usr/bin/python
####
cd /usr/bin/ # 进入 bin 文件夹
ll python*  # 列出包含 python 字段的文件
>>> 
lrwxrwxrwx. 1 root root   16 11月 17 11:42 python -> /usr/bin/python2
lrwxrwxrwx. 1 root root    9 11月 13 11:01 python2 -> python2.7
-rwxr-xr-x. 1 root root 7144 10月 14 22:46 python2.7
-rwxr-xr-x. 1 root root 1835 10月 14 22:45 python2.7-config
lrwxrwxrwx. 1 root root   16 11月 16 17:40 python2-config -> python2.7-config
lrwxrwxrwx. 1 root root   14 11月 16 17:40 python-config -> python2-config
>>>
# 可以看出 python 指向 python2，最终指向 python2.7
```

# 编译安装 Python

```shell
# 安装软件依赖以编译文件
yum -y install zlib zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel libffi-devel gcc-c++ perl
# 下载 Python 源码，解压，然后进入文件夹，编译到对应目录，安装
wget https://mirrors.aliyun.com/python-release/source/Python-3.6.0.tgz
tar -zxvf Python-3.6.0.tgz
cd Python-3.6.0 || { echo "Failure"; exit 1; }
./configure --prefix=/usr/local/python3
# 注意，当需要使用 SSL 时，应当添加参数 --with-openssl=/usr/local/openssl
# 注意，当需要打包编译 py 文件时，应当添加参数 --enable-shared
# 使用参数 --enable-shared 编译完成后，需要额外链接 libpython3.9.so.1.0 文件
# ln -s /usr/local/python3/lib/libpython3.9.so.1.0 /usr/lib64/libpython3.9.so.1.0
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

# 安装 openssl

在 Python 3.6 以后的版本中，pip 以及部分网络连接需要使用到 SSL 协议，此时，如果 Python 在编译时没有使用 openssl 库，则在使用网络连接时会出现报错。

同时，需要注意的是，Python 支持 openssl 版本需要大于 1.1.1。

```shell
# 下载源代码文件
wget https://www.openssl.org/source/openssl-1.1.1.tar.gz
# 解压
tar -zxvf openssl-1.1.1.tar.gz
cd openssl-1.1.1
# 编译
./config --prefix=/usr/local/openssl shared zlib
# 安装
make && make install
# 将新版本的 openssl 链接到环境中
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl
ln -s /usr/local/openssl/include/openssl /usr/include/openssl
echo "/usr/local/openssl/lib" >> /etc/ld.so.conf
ldconfig -v
ldconfig -p | grep ssl
# 检查
openssl version
# 在 Python 编译时使用
./configure --prefix=/usr/local/python3 --with-openssl=/usr/local/openssl
```

# 安装 pyinstaller

pyinstaller 编译程序需要在编译 Python 时携带 `--enable-shared` 参数，否则程序会无法找到动态链接库，进而报错。

同时，需要注意的是，在携带 `--enable-shared` 编译成功后，用户需要手动连接生成的 `libpython3.9.so.1.0` 文件，使用命令：`ln -s /usr/local/python3/lib/libpython3.9.so.1.0 /usr/lib64/libpython3.9.so.1.0`

```shell
pip install pyinstaller
ln -s /usr/local/python3/bin/pyinstaller /usr/bin/pyinstaller
```

# 将 python 连接到 python3

```shell
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
