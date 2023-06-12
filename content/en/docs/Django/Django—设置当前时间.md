---
title: Django 在 model 中设置当前时间
date: 2021-06-03
author: LM
---

## 1.model 的时间字段

在 model 中，有 DateTimeField、DateField 和 TimeField 三种类型可以用来创建日期字段，其值分别对应着 datetime()、date()、time() 三种对象。

## 2.可用属性

- `DateTimeField.auto_now`：这个参数的默认值为 false，设置为 true 时，能够在保存该字段时，将其值设置为当前时间，并且每次修改 model，都会自动更新。需要注意的是，设置该参数为 true 时，并不简单地意味着字段的默认值为当前时间，而是指字段会被强制更新到当前时间，你无法在程序中手动为字段赋值，该字段是只读的。
- `DateTimeField.auto_now_add`：这个参数的默认值也为 false，设置为 true 时，会在 model 对象第一次被创建时，将字段的值设置为创建时的时间，以后修改对象时，字段的值不会再更新。与 auto_now 类似，auto_now_add 也具有强制性，一旦被设置为 true，就无法在程序中手动为字段赋值。

## 3.如何将创建时间设置为默认当前并且可修改

在 Django 中，所有的 model 字段都拥有一个 default 参数用来给字段设置默认值。因此可以用 `default=timezone.now` 来替换 `auto_now=True` 或 `auto_now_add=True`来解决创建时间不可变的问题。

```python
from django.db import models
import django.utils.timezone as timezone

class Doc(models.Model):
	add_date = models.DateTimeField('保存日期',default = timezone.now)
	mod_date = models.DateTimeField('最后修改日期', auto_now = True)
```