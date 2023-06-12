---
title: auth 用户认证模块
date: 2021-07-26
author: LM
---

## 1.介绍

Django 内置了强大的用户认证系统`auth`，它默认创建并使用`auth_user`表来存储用户数据。

```python
from django.contrib import auth  # 使用auth认证系统
from django.contrib.auth.models import User  # auth认证系统默认使用User表
```

## 2.auth.authenticate() 

Django 提供简单的用户认证功能，如果认证成功（用户名和密码正确有效），便会返回一个`User`对象。

```python
from django.contrib import auth
user_obj = auth.authenticate(username=username,password=pwd)
```

## 3.auth.login(request, user)

该函数实现一个用户登录的功能，它本质上会在后端为该用户生成相关 session 数据。在使用`login(request, user_obj)`登录后之后，便可以通过`request.user`拿到当前登录的用户对象，否则`request.user`得到的是一个匿名用户对象。

```python
from django.shortcuts import render, HttpResponse, redirect
from django.contrib import auth

def login(request):
    if request.method == "POST":
        username = request.POST.get('username')
        pwd = request.POST.get('password')
        # 调用auth模块的认证方法，判断用户名和密码是否正确，正确返回一个user_obj
        user_obj = auth.authenticate(username=username, password=pwd)
        if user_obj:
            # 登录成功,设置Session数据
            auth.login(request, user_obj)
            return HttpResponse('登录成功')
        else:
            return render(request, 'login.html', {'error_msg': '用户名或者密码错误'})
    return render(request, 'login.html')
```

## 4.auto.logout(request) 

该函数会将当前请求的 session 信息会全部清除。

```python
from django.contrib import auth
   
def logout(request):
    auth.logout(request)
    return redirect('/login/')
```

## 5.@login_required

该装饰器可以便捷的为某个视图添加登录校验，若用户没有登录，则会跳转到默认的登录界面并传递当前访问界面的绝对路径 (登陆成功后，会重定向到该路径)。如果需要自定义登录的URL，则需要在`settings.py`文件中通过`LOGIN_URL = '/login/'`进行修改。

```python
from django.contrib.auth.decorators import login_required
      
@login_required
def index(request):
    return render(request, 'index.html')
```

## 6.create_user()

该函数可以创建一个新用户，注意创建时明文输入的密码会在数据库中加密存在。

```python
from django.contrib.auth.models import User
user = User.objects.create_user(username='用户名',password='密码',email='邮箱',...)

def reg(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        pwd = request.POST.get('password')
        # 假设数据都经过有效性校验了
        # 去数据库创建一条记录
        User.objects.create_user(username=username, password=pwd)  
        # create_user创建普通用户
        # User.objects.create_superuser(username=username, password=pwd)  
        # create_superuser创建超级用户
        # 创建成功，跳转登录页年
        return redirect('/login/')
    return render(request, 'reg.html')
```

## 7.check_password(raw_password)

该函数可以对密码的正确与否进行检查。

```python
ok = user_obj.check_password('密码')
# 或者直接针对当前请求的user对象校验原密码是否正确：
ok = request.user.check_password(raw_password='原密码')
```

## 8.is_authenticated()

该函数可以判断用户是否通过验证。

```python
def my_view(request):
  if not request.user.is_authenticated():
    return redirect('%s?next=%s' % (settings.LOGIN_URL, request.path))
```

## 9.set_password()

该函数能实现对密码的修改，注意在不能直接的查找密码修改，这是因为在数据库中用户的密码是以加密形式存在的，`auth`的验证是先接收明文密码，在加密后进行验证，如果直接修改则明文加密后与数据库的明文密码便对接不上。

```python
request.user.set_password(pwd)
request.user.save()  # 修改密码
```

## 10.扩展auto默认表

由于默认的`auth_user`表字段都是固定的，如果用户需要添加别的字段，可以这样操作：

- 方法一：新建另外一张表然后通过一对一和内置的auth_user表关联。
- 方法二：通过继承内置的AbstractUser类，来定义一个自己的Model类。

```python
models.py
from django.db import models
from django.contrib.auth.models import AbstractUser


class UserInfo(AbstractUser):
    # 这里定义拓展的字段
    gender = models.PositiveIntegerField(choices=((0, '女'),(1, '男'), (2, '保密')))
    phone = models.CharField(max_length=11)
```

需要注意的是，在扩展表后一定要在`settings.py`中告诉Django现在使用新定义的`UserInfo`表来做用户认证，`AUTH_USER_MODEL = "app名.UserInfo"`。

PS：当自定义用户表与框架自动生成的用户表发生冲突时，即执行 `migrate` 命令同步数据库时出现错误，此时可以打开`settings.py`注释掉`INSTALL_APPS`中的`django.contrib.admin`，然后再次同步便可以解决该问题。为避免出现以上的问题，建议在创建数据库前先把自定义的用户表定义好。

{{< details "参考文件" >}} 
1：[ Django自带的用户认证auth模块  @Zzbj ](https://www.cnblogs.com/Zzbj/p/9984783.html)
{{< /details >}}