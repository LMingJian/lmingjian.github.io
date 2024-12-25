---
title: JS 中 attribute 和 property 的区别
date: 2024-12-25T16:17:02+08:00
author: LiangMingJian
---

# 定义

`property`和`attribute`虽然都称为属性，但两者不是同一个东西，`property`是 DOM 中的属性，是 JavaScript 里的对象；

`attribute`是 HTML 标签上的属性，它的值只能够是字符串。比如`attribute`就是 DOM 节点自带的属性，例如 HTML 中常用的`id、class、title、align`等。

`property`是这个 DOM 元素作为对象，其附加的内容，例如`childNodes、firstChild`等。

```html
<div id="div1" class="divClass" title="divTitle" title1="divTitle1"></div>
```

如上述的`div`标签，当我们使用`var in1=document.getElementById("div1")`获取这个元素并打印时`console.log(in1)`，可以看到如下图的属性。

![](/_images/drawingbed/img/202205051102194.png)

可以发现有一个名为`attributes`的属性，类型是`NamedNodeMap`，同时也可以找到标签自带的属性`id`和`className`、但明显没有`titles`这个自定义的属性。这是因为每一个 DOM 对象在创建的时候，只会创建如`id, className`这些基本属性，而们在 TAG 标签中自定义的属性是不会直接放到 DOM 中的。

![](/_images/drawingbed/img/202205051102155.png)

从这里就可以看出，**`attributes`是属于`property`的一个子集**。

# 区别一：attribute 取值赋值

`attribute` 使用`setAttribute()`和`getAttribute()`进行操作，注意，`setAttribute()`的两个参数，都必须是字符串。

```javascript
var id = div.getAttribute("id");              
var className = div.getAttribute("class");
var title = div.getAttribute("title");
var value = div.getAttribute("value");   // 自定义属性
div.setAttribute('class', 'a');
div.setAttribute('title', 'b');
div.setAttribute('title', 'c');
div.setAttribute('value', 'd');
```

# 区别二：property 取值赋值

`property`取值赋值只需要使用点`.`就可以了。对属性`property`可以赋任何类型的值。

```javascript
var id = div.id;
var className = div.className;
var childNodes = div.childNodes;
var attrs = div.attributes;
div.className = 'a';
div.align = 'center';
div.AAAAA = true;
div.BBBBB = [1, 2, 3];
```

# 两者的联系

- `property`能够从`attribute`中得到同步；
- `attribute`不会同步`property`上的值；
- `attribute`和`property`之间的数据绑定是单向的，`attribute->property`；
- 更改`property`和`attribute`上的任意值，都会将更新反映到 HTML 页面中；

```javascript
// 如下，更改 property 的值不能更改 attribute 的值
in1.value='new value of prop';
console.log(in1.value);               // 'new value of prop'
console.log(in1.attributes.value);    // 1
// 相反，更改 attribute 的值可以更改 property 的值
in2.setAttribute('value','ni')
console.log(in2.value);          // ni
console.log(in2.attributes.value); // value = 'ni'
```

{{< details "参考文件" >}} 
1：[ JS 中 attribute 和 property 的区别  @L_mj ](https://www.cnblogs.com/lmjZone/p/8760232.html)
{{< /details >}}
