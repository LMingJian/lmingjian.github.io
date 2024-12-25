---
title: CSS 选择器
date: 2024-12-25T16:13:45+08:00
author: LiangMingJian
---

# 概述

CSS 选择器 是 CSS（层叠样式表）提供的一种 HTML 元素定位选择工具，可以用来选择你想要的元素。

# 标签选择器

用 HTML 标签名称作为选择器，按标签名如 a，div，ul 等进行分类。

```
tag {
    name: value;
}
```

# 类选择器

使用标签中加入的 `class='class_name'` 属性进行选择分类。

```
.class_name {
    name: value;
}
```

# ID 选择器

使用标签中唯一的 `id='id_name'` 属性进行选择分类。

```
#id_name {
    name: value;
}
```

# 通配符选择器

使用 CSS 中通配符来进行选择分类，比如选取页面中所有的标签的通配符 `*`。

```
* {
    name: value;
}
```

# 父子选择器(派生选择器)

根据 HTML 元素包含关系，对内层元素进行选择。

```
最外层 外层 内层{
    name: value;
}
```

比如需要选中下面 p 标签内的 span，可以使用 `div span` 进行。

```html
<div>
    <p>
        <span>123</span>
    </p>
</div>
<span>123</span>
```

# 直接子元选择器

只能选择最近一级选择器，控制的是下一层的某个元素。

```
外层 > 内层{
    name: value;
}
```

比如需要选中下面 div 标签内的第一个 em 元素，可以使用 `div > em` 进行。

```html
<div>
    <em>123</em>
    <strong>
        <em>234</em>
    </strong>
</div>
```

# 链接伪类选择器

- `a:link`：超链接点击前，选择所有未访问的链接
- `a:visited`：访问过的，选择所有已经访问的链接
- `a:hover`：悬停时，鼠标指针位于该链接上显示
- `a:active`：激活，鼠标点击但是没松手时

# 结构伪类选择器

- `selector:first-child`：用来定位一组兄弟元素中的第一个元素
- `selector:last-child`：用来定位一组兄弟元素中的最后一个元素
- `selector:nth-child(n)`：用来定位一组兄弟元素中的第 n 个元素
- `selector:nth-last-child(n)`：用来定位一组兄弟元素中倒序方式的第 n 个元素
- `selector:first-of-type`：用来定位一组同类型的兄弟元素中的第一个元素
- `selector:last-of-type`：用来定位一组同类型的兄弟元素中的最后一个元素
- `selector:nth-of-type(n)`：用来定位一组同类型的兄弟元素中的第 n 个元素
- `selector:nth-last-of-type(n)`：用来定位一组同类型的兄弟元素中倒序方式的第 n 个元素

# 否定伪类选择器

- `selector:not(selector)`：在选择时，除了某一个标签外的其他元素都选择

{{< details "参考文件" >}} 
1.[ CSS选择器  @Aurora ](https://blog.csdn.net/qq_62755767/article/details/124229255)
{{< /details >}}
