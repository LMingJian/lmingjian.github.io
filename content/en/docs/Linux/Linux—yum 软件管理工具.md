---
title: Linux yum 软件管理工具
date: 2020-11-19
author: LM
---

## 1.yum 的源码安装

yum（ Yellow dog Updater, Modified ）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。yum 提供了查找、安装、删除某一个、一组甚至全部软件包的命令。

```bash
# 新建一个目录用来保存yum安装包 
mkdir install
# 进入文件夹并输入命令
cd install
wget http://yum.baseurl.org/download/3.2/yum-3.2.28.tar.gz
# 解压
tar -xvf yum-3.2.28.tar.gz
# 重点：解压后先不着急安装，手动创建一个yum的conf文件，不然会报找不到文件的错 yum.cli:Config Error: Error accessing file for config file:///etc/
# touch /etc/yum.conf
# 进入yum目录，脚本安装
cd yum-3.2.28
./yummain.py install yum
# 期间会提示安装新版本，y回车即可
```

## 2.使用

```bash
yum -y remove xxxx     #卸载
yum -y install xxxxx   #安装
```

