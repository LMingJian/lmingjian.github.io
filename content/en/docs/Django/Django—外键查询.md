---
title: Django 在 model 中查询外键
date: 2021-07-24
author: LM
---

## 1.外键查询

在 Django 中，`ForeignKey`的查询可以使用类属性或双下划线进行。

### a.通过类属性查询

```python
class Publish(models.Model):
    id = models.AutoField(primary_key=True, auto_created=True)
    pname = models.CharField(max_length=40)
    city = models.CharField(max_length=50)

    def __str__(self):
        return self.pname

class Author(models.Model):
    id = models.AutoField(primary_key=True, auto_created=True)
    aname = models.CharField(max_length=10)

    def __str__(self):
        return self.aname

class Book(models.Model):
    id = models.AutoField(primary_key=True, auto_created=True)
    bname = models.CharField(max_length=30)
    price = models.IntegerField()
    publish = models.ForeignKey(Publish, on_delete=models.CASCADE)
    author = models.ManyToManyField(Author)

    def __str__(self):
        return self.bname

# get方法的到的结果是一个对应类的对象
# 查询某本书的出版社名字
book = Book.objects.get(id=1)
book.publish.pname

# 查询某出版社下面有多少本书
# 此处的book是Book这张表的表名的小写（必须是小写）加上_set
pub = Publish.objects.get(id=1)
pub.book_set.all()
```

### b.通过双下划线查询

```python
# 通过出版社的相关信息进行查询某一本书
Book.objects.filter(publish__city='北京')
Book.objects.filter(publish__id=1)

# 通过书籍的相关信息进行查询其出版社
# 此处的book是Book这张表的表名的小写（必须是小写）
Publish.objects.filter(book__id=1)

# 在values以及values_list中使用（必须加引号）
# 通过书籍的相关信息进行查询其出版社
# values得到的结果是一个内部是字典的查询集
Book.objects.filter(id=1).values('publish__pname')
# values__list得到的结果是一个内部是元祖的查询集
Book.objects.filter(id=1).values_list('publish__pname')

# 通过出版社的相关信息进行查询某一本书
Publish.objects.filter(id=1).values('book__bname')
Publish.objects.filter(id=1).values_list('book__bname')
```

{{< details "参考文件" >}} 
1：[ Django 外键查询 @尛刀石 ](https://blog.csdn.net/qq_43102443/article/details/108270653)
{{< /details >}}