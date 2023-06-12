---
title: models
date: 2021-06-01
author: LM
---

## 1.models 的基本概念

Django 中每个 models 都是一个 Python 类，这些类继承自 `django.db.models.Model`。模型类的每个属性都相当于一个数据库的字段。

```python
from django.db import models
class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

上述代码相当于使用数据库语言创建一个 Person 表单，有两个字段 first_name 与 last_name。

```sql
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

## 2.使用模型

在定义模型后，你需要告诉 Django 将使用这些模型。通过编辑你的设置文件 setting.py，改变 INSTALLED_APPS 设置来添加包含你 models.py 的模块的名称来实现这一点。

```python
INSTALLED_APPS = [
    #...
    'myapp', # 模块名
    #...
]
```

## 3.字段

### a.字段类型

略

### b.字段选项

- max_length：该参数指定用于存储数据的 VARCHAR 数据库字段的大小。
- null：如果是 True，Django 会将空置的值存储为 NULL。默认是 False。
- blank ：如果是 True，这个字段是空白的。默认是 False。注意，这与 null 不同的是，null 与数据库相关，而 blank 则是与验证相关的。如果一个字段有 blank=True ，表单验证就允许输入空值。如果一个字段有 blank=False ，则需要字段。
- choices：2元组的可迭代，例如，列表或元组的元素选择。
- default：字段的默认值。
- unique：如果是真的，这个字段必须在整个表中是唯一的。
- related_nam：使用外键时调用的名字。

### c.自动主键字段

在默认情况下，Django默认提供了主键字段：`id = models.AutoField(primary_key=True)`

这是一个自动递增的主键。如果您想要指定一个定制的主键，请在您的一个字段中指定 primarykey=True。如果 Django 看到你已经明确地设置了主键，那它便不会添加自动 id。

## 4.关联关系

显然，关系数据库的功能在于将表相互关联起来。Django 提供了定义三种最常见的数据库关系类型的方法：多对一、多对多和一对一。

注意：某些版本必须给定 on_delete 属性值才能创建关联关系。

- models.CASCADE(默认选项)：当 Wife 关联的 Husband 被删除时，Wife 也会被一同删除，反之，Husband 不会被删除。
- models.PROTECT(保护选项)：设置该选项后，若要删除 Husband 的数据，且该数据关联 Wife 中的数据，此时系统会报错提示数据受保护，反之可以正常删除。
- models.SET_NULL(空值选项)：设置该选项后，若要删除 Husband 的数据，且该数据关联 Wife 中的数据，可正常删除，且 Wife 不会被删除，此时会把 Wife 的关联数据设置为空，注意使用此选项时必须设置 null=True（允许空值），否则会出现异常。
- models.SET_DEFAULT(默认值选项)：该选项与 SET_NULL 类似，当删除与 Wife 绑定的 Husband 时，会把 Wife 的关联数据设置为默认值，注意使用此选项时必须设置 Default=默认值，否则会出现异常。
- models.SET()：当删除与 Wife 绑定的 Husband 时，会把 Wife 的关联数据设置为括号内的值，注意防止设置的值与其他设置冲突。

### a.多对一

字段 ForeignKey

例如，如果一个汽车模型有一个制造商，也就是说，制造商生产多辆汽车，但每辆车都只有一个制造商，可以使用以下定义：

```python
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
    # ...
```

### b.多对多

字段 ManyToManyField

例如，一个 pizza 有多个 topping 的对象，而一个topping也可以在多个 pizza 上，使用以下定义：

```python
class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)
```

### c.一对一

字段 OneToOneField

事实上，处理这通常使用继承，它涉及隐式的一对一关系，这里不展开细讲。

## 5.Meta 选项

Django 模型类的 Meta 是一个内部类，它用于定义一些 Django 模型类的行为特性。

- abstract：定义当前的模型是不是一个抽象类。所谓抽象类是不会对应数据库表的。一般我们用它来归纳一些公共属性字段，然后继承它的子类可以继承这些字段。
- get_latest_by：指定一个 DateField 或者 DateTimeField。这个设置让你在使用 model 的 Manager 上的 lastest 方法时，默认使用指定字段来排序。
- ordering：这个字段是告诉 Django 模型对象返回的记录结果集是按照哪个字段排序的。这是一个字符串的元组或列表，没有一个字符串都是一个字段和用一个可选的表明降序的'-'构成。当字段名前面没有'-'时，将默认使用升序排列。使用'?'将会随机排列。
- verbose_name：给模型类起一个更可读的名字，一般定义为中文。
- verbose_name_plural：指定模型的复数形式是什么。

```python
class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
        ordering=['order_date'] # 按订单升序排列
        ordering=['-order_date'] # 按订单降序排列，-表示降序
        ordering=['?order_date'] # 随机排序，？表示随机
        ordering=['-pub_date','author'] # 以pub_date为降序，在以author升序排列
```

## 6.模型方法

`__str__()`：Python“魔术方法”，返回任何对象的字符串表示形式。这是 Python 和 Django 在模型实例需要被强制并显示为纯字符串时将使用的内容。

```python
class Topping(models.Model):
    # ...
    
    def __str__():
        print('Topping')

```

## 7.模型属性

模型最重要的属性是 Manager。使用 Manager 定义 objects，为 Django 模型提供数据库查询操作的接口，用于从数据库中检索实例。如果 Manager 未定义，管理员只能通过模型类访问，而不能通过模型实例访问。

```python
objects = models.manager
```

## 8.增删查改

### 增

```python
models.UserInfo.objects.create(user='yangmv',pwd='123456')
obj = models.UserInfo(user='yangmv',pwd='123456')
obj.save()
dic = {'user':'yangmv','pwd':'123456'}
models.UserInfo.objects.create(**dic)
```

### 删

```python
models.UserInfo.objects.filter(user='yangmv').delete()
```

### 查

```python
models.UserInfo.objects.all()
models.UserInfo.objects.all().values('user')  # 只取 user 列
models.UserInfo.objects.all().values_list('id','user')  # 取出 id 和 user 列，并生成一个列表
models.UserInfo.objects.get(id=1)
models.UserInfo.objects.get(user='yangmv')
models.Tb1.objects.filter(id__in=[11, 22, 33])  # 获取 id 等于 11、22、33 的数据
models.Tb1.objects.exclude(id__in=[11, 22, 33]) # not in
```

### 改

```python
models.UserInfo.objects.filter(user='yangmv').update(pwd='520')
obj = models.UserInfo.objects.get(user='yangmv')
obj.pwd = '520'
obj.save()
```