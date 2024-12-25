---
title: 插件 jsMind 的多行展示实现
date: 2024-12-25T16:15:00+08:00
author: LiangMingJian
---

# 需求

[jsMind](https://www.npmjs.com/package/jsmind) 是一个显示/编辑思维导图的纯 javascript 类库，其基于 html5 canvas 和 svg 进行设计。jsMind 以 [BSD 协议开源](https://github.com/hizzgdev/jsmind/blob/HEAD/LICENSE)，在此基础上你可以在你的项目上任意使用。

思维导图插件 jsMind 的节点默认以一行展示所有内容，当数据过多时不会进行折行，过多的数据会被折叠。

# 修改

通过修改 **jsmind.css** 里如下内容，可以使得节点数据的展示允许自动折行。

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
