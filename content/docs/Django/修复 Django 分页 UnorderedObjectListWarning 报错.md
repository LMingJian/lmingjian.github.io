---
title: 修复 Django 分页 UnorderedObjectListWarning 报错
date: 2024-12-25T11:07:36+08:00
author: LiangMingJian
---

# BUG 描述

Django 分页时出现报错。

```python
UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <class 'sign.models.Guest'> QuerySet.paginator = Paginator(guest_list,5)
```

# Resolution

这是因为 Django 分页是依照排序进行的，而未排序时进行分页便会发生该报错。

我们需要定位到分页依据的数据，然后对该数据进行排序。

```python
paginator = Paginator(gList, 5)
gList = G.objects.all().order_by('id')
```
