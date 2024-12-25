---
title: Linux Tool：rpm
date: 2024-12-25T13:59:16+08:00
author: LiangMingJian
---

# 概述

在 Linux 操作系统下，几乎所有的软件都可以通过 rpm 进行安装、卸载及管理等操作。rpm 的全称为 Redhat Package Manager ，是由 Redhat 公司提出的，用于管理 Linux 下软件包的软件。

# 安装

```bash
rpm -i example.rpm  # 安装 example.rpm 包；
rpm -iv example.rpm # 安装 example.rpm 包并在安装过程中显示正在安装的文件信息；
rpm -ivh example.rpm # 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度
```

# 删除

```bash
rpm -e example  # 注意：软件包名是example，而不是 rpm 文件名 example.rpm
```

# 升级

```bash
rpm -Uvh example.rpm
# 注意：rpm 会自动卸载相应软件包的老版本。如果老版本软件的配置文件与新版本的不兼容，rpm 会自动将其保存为另外一个文件，用户会看到下面的信息：saving /etc/example.conf as /etc/example.conf.rpmsave，用户就可以自己手动去更改相应的配置文件
# 注意：如果用户要安装老版本的软件，会看到下面的出错信息：error:example.rpm cannot be installed，强行安装要使用 -oldpackage 参数。
```

# 查询

```bash
rpm -qa | grep clickhouse
# 用户可以用 rpm -q 在 rpm 的数据库中查询相应的软件，rpm 会给出软件包的名称，版本，发布版本号
----- 可用参数 ---------------------------------------------
-a    # 查询目前系统安装的所有软件包。
-f file     # 查询包括该文件的软件包。
-F    # 同 -f 参数，只是输入是标准输入
-q package  # 查询该软件包
-Q    # 同 -p 参数，只是输入是标准输入
-i    # 显示软件包的名称，描述，发行，大小，编译日期，安装日期，开发人员等信息。
-l    # 显示软件包包含的文件
-s    # 显示软件包包含的文件目前的状态，只有两种状态：normal 和 missing
-d    # 显示软件包中的文档（如 man,info,README 等）
-c    # 显示软件包中的配置文件
-v    # 类似于ls -l的输出
```

# 校验

```bash
rpm -Vf package
rpm -Va  # 全部校验 
```
