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

## 3.补充：镜像切换

由于 CentOS 停止维护，Yum 镜像无法使用，因此需要设置镜像源来提高软件包安装和更新的速度。

首先备份你当前的 YUM 仓库配置。

```sh
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

然后编辑 CentOS-Base.repo 文件，将文件中的 baseurl 和 mirrorlist 行注释掉或者删除，并替换成你想要的镜像地址。

```shell
sudo vi /etc/yum.repos.d/CentOS-Base.repo
```

当然，你也可以将以下内容复制并粘贴到你的 CentOS-Base.repo 文件中，来使用阿里云的镜像源。

```
[base]
name=CentOS-$releasever - Base - Aliyun
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
 
#released updates 
[updates]
name=CentOS-$releasever - Updates - Aliyun
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
 
#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras - Aliyun
baseurl=http://mirrors.aliyun.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
 
#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus - Aliyun
baseurl=http://mirrors.aliyun.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
```

最后保存文件并退出编辑器，清除 YUM 缓存并重新生成缓存，现在你可以使用 YUM 进行软件包的安装和更新了。

```shell
sudo yum clean all
sudo yum makecache
```