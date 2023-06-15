---
title: Linux LAMP 环境配置
date: 2020-11-18
author: LM
---

## 1.什么是 LAMP 环境

LAMP 是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写。

- Linux，操作系统
- Apache，网页服务器
- MariaDB或MySQL，数据库管理系统（或者数据库服务器）
- PHP、Perl或Python，脚本语言

## 2.MariaDB 数据库

```bash
yum -y mariadb mariadb-server  #安装mariadb客户端和服务端程序
yum groupinstall mariadb mariadb-server -y  #或
systemctl start mariadb         #启动程序
systemctl enable mariadb         #设为开机自启动
mysql_secure_installation         #直接执行初始化命令，会弹出交互配置信息
##交互信息##
Enter current password for root (enter for none):#初次进入密码为空，直接回车
New password:                #输入要为root用户设置的数据库密码。
Re-enter new password:            #重复再输入一次密码。
Remove anonymous users? [Y/n] y      #删除匿名帐号
Disallow root login remotely? [Y/n] n #是否禁止root用户从远程登录，安全起见应禁止，这里为做实验方便这里不禁止。
Remove test database and access to it? [Y/n] y  #是否删除test数据库，想留着也随意
Reload privilege tables now? [Y/n] y  #刷新授权表，让初始化后的设定立即生效mariadb中命令都要以";" 结尾，表示命令输入完毕
##交互结束##
mysql -uroot -p123456  #登录
show databases;  #显示当前已有的数据库
show tables;   #显示当前数据库中的表单
desc user;     #查看user表的数据结构
select * from user;   #查询mysql库下的user表中的所有
create database lan;      #创建库
create table Linux （         #创建表格
           username varchar（50） not null，  #字段名称 字段长度50 不能为空
           password  varchar（50） not null，
           age  varchar（4） ）；
insert into Linux values ('we','123','23');  #在Linux表格中插入信息
use 数据库; #使用数据库
drop table Linux；#删除表格Linux
drop database lan； #删除库lan
exit
#ps：如果出现无权限错误，请尝试rm -rf /var/lib/mysql/*，删除所有mysql，重新安装
```
## 3.Httpd与php

```bash
yum -y install httpd php php-mysql php-gd #安装httpd 与 php
systemctl start httpd #启动httpd，请开启80端口，见Linux(2)——Docker安装与防火墙
vim /var/www/html/test.php  #创建测试页面，<?php phpinfo();?>，处于/var/www/html/目录下html文件都可以访问
```

