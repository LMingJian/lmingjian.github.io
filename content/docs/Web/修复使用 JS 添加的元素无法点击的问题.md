---
title: 修复使用 JS 添加的元素无法点击的问题
date: 2024-12-25T16:16:12+08:00
author: LiangMingJian
---

# BUG 描述

`javascript` 在使用`append()`添加元素后，该元素的点击事件无法被监听生效。

# Resolution

由于动态加载的元素是在`css, js`代码加载完后才添加的，因此当浏览器在解析`javascript` 代码时，这些动态添加的元素并未生成，从而无法绑定相应的事件，完成对事件进行监听。

因为`body`这个元素是最早加载的一个元素，因此我们可以通过对`body`绑定事件来完成目标元素事件的绑定。

```javascript
$("body").on("click", '.addBtn', function(){
    alert('new')
})
```

{{< details "参考文件" >}} 
1：[ javascript 添加 HTML 元素时出现无效的点击事件 @wttwuhn ](https://juejin.cn/post/6844903703896391687)
{{< /details >}}
