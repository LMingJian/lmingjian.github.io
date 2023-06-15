---
title: Linux 配置 Java 与 Tomcat 环境
date: 2020-10-22
author: LM
---

## 1.Java 配置

在配置 Java 环境时，需要跟随内核版本，ARM 下 ARM，x86 下 x86，从下方链接中下载源码。

[ JDK 官网下载 ](https://www.oracle.com/java/technologies/javase-downloads.html) 

```bash
# 查看内核
arch
uname -a
# 删除自带的openjava
rpm -qa | grep java
yum -y remove openjava
# 配置环境变量
vim /etc/profile
# 添加以下内容
# ------------------
JAVA_HOME=/home/ams/jdk
PATH=$PATH:${JAVA_HOME}/bin
export JAVA_HOME PATH
# ------------------
source /etc/profile # 启用环境，配置后启动环境，若配置环境后无法使用Java，需运行此命令
java -version
```

## 2.Tomcat 配置

注意跟随内核版本，从下方链接中下载源码。

[ Tomcat 官网下载 ](https://tomcat.apache.org/) 

```bash
# 配置环境变量，打开startup.sh和shutdown.sh，添加以下内容
export TOMCAT_HOME=/home/ams/tomcat
export CATALINA_HOME=/home/ams/tomcat
export PATH=$PATH:/home/ams/tomcat/bin
# --------------------------------------
/bin/bash startup.sh # 运行tomcat
ps -ef | grep tomcat # 是否安装tomcat，浏览器打开 IP:8080，将 Html 文件放置在 Tomcat 目录下 webapps 文件夹内，即可访问相应界面
```

