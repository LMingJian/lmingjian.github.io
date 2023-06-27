---
title: 思维导图插件 jsMind 多行展示实现
date: 2022-06-13
author: LM
---

## 1.需求

思维导图插件 jsMind 的节点默认以一行展示所有内容，当数据过多时不会进行折行，过多的数据会被折叠。

## 2.修改

修改 **jsmind.css** 里如下内容，使得节点数据的展示允许自动折行。

```css
jmnodes{
    position:absolute;
    z-index:2;
    background-color:rgba(0,0,0,0);
    min-width:420px;
}
jmnode{
    position:absolute;
    cursor:default;
    max-width:400px;
}
```

