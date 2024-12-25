---
title: Django 的命令行使用
date: 2024-12-25T11:07:52+08:00
author: LiangMingJian
---

# manage.py

`manage.py`是 Django 项目中的一个管理脚本，能帮助用户完成诸如数据库初始化，项目启动，用户创建等工作。

# Django 的安装

```python
pip install django
python -m django --version
```

# 创建项目

```python
django-admin startproject 项目名称
python manage.py startapp 应用名称
```

# 启动调试服务器

```python
python manage.py runserver
python manage.py runserver 8080
python manage.py runserver 0.0.0.0:8000
```

# 初始化数据库

```python
python manage.py makemigrations 应用名称 #记录改动
python manage.py migrate #创建表
python manage.py createsuperuser
```

# 创建用户

```python
python manage.py shell
>>> from django.contrib.auth.models import User
>>> user=User.objects.create_user(username='user1',password='12345678')
```
