---
title: Django 的 cookie 的处理
date: 2021-06-04
author: LM
---

## 1.设置 cookie

在 Django 中，用户可以通过`set_cookie`方法来自行设置 cookie 参数。

```python
# 编写视图函数，进行设置
from datetime import datetime,timedelta
def set_cookie(request):
    """设置cookie"""
    response = HttpResponse("设置cookie")
    ''' max_age 设置过期时间，单位是秒 '''
    # response.set_cookie('name', 'tong', max_age=14 * 24 * 3600)
    ''' expires 设置过期时间，是从现在的时间开始到那个时间结束 '''
    response.set_cookie('name', 'tong', expires=datetime.now()+timedelta(days=14))
    return response
```

## 2.获取cookie

```python
# 视图函数中定义  get_cookie 方法
def get_cookie(request):
    """获取cookie"""
    name = request.COOKIES['name']
    return HttpResponse(name)
```

