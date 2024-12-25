---
title: 如何在 Django 的模板中遍历字典
date: 2024-12-25T11:07:02+08:00
author: LiangMingJian
---

# 遍历字典

在模版中要遍历字典 dict ，一般使用如下代码实现。

```html
{% for key,value in param.items %} 
    {{ key }}
    {{ value }}
{% endfor %}
```
