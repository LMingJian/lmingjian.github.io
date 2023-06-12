---
title: models 属性 on_delete
date: 2022-01-24
author: LM
---

## 1.on_delete 的值

在 Django 的数据库模型中，外键等关联字段必须有 `on_delete` 属性，该属性允许取如下的值。

```python
on_delete=None,               # 删除关联表中的数据时,当前表与其关联的field的行为
on_delete=models.CASCADE,     # 删除关联数据,与之关联也删除
on_delete=models.DO_NOTHING,  # 删除关联数据,什么也不做
on_delete=models.PROTECT,     # 删除关联数据,引发错误ProtectedError
# models.ForeignKey('关联表', on_delete=models.SET_NULL, blank=True, null=True)
on_delete=models.SET_NULL,    # 删除关联数据,与之关联的值设置为null（前提FK字段需要设置为可空,一对一同理）
# models.ForeignKey('关联表', on_delete=models.SET_DEFAULT, default='默认值')
on_delete=models.SET_DEFAULT, # 删除关联数据,与之关联的值设置为默认值（前提FK字段需要设置默认值,一对一同理）
on_delete=models.SET,         # 删除关联数据,
# a. 与之关联的值设置为指定值,设置：models.SET(值)
# b. 与之关联的值设置为可执行对象的返回值,设置：models.SET(可执行对象)
```
