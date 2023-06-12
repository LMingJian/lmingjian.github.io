---
title: 在模板中遍历字典
date: 2021-06-02
author: LM
---

## 1.遍历字典

在模版中要遍历字典 dict ，一般使用如下代码实现。

```html
{% for key,value in param.items %} 
    {{ key }}
    {{ value }}
{% endfor %}
```

