---
title: CSS 选择器使用手册
date: 2024-12-25T16:13:45+08:00
author: LiangMingJian
---

# 概述

CSS 选择器是 CSS（层叠样式表）提供的一种 HTML 元素定位选择工具，该工具常用于 CSS 文件编写和 Selenium 浏览器自动化。

# 标签选择器

使用 HTML 标签名（a，div，ul）作为选择器定位元素。

在使用时，直接使用标签名进行标识。

**CSS 代码**

```css
div {
    color: orange;
}
```

**Selenium 代码**

```python
element = driver.find_element(By.CSS_SELECTOR, "div")
```

# 类选择器

使用标签中的 `class='name'` 属性进行元素定位。

在使用时，通过点 `.` 加类名进行标识。

**CSS 代码**

```css
.name {
    color: orange;
}
```

**Selenium 代码**

```python
element = driver.find_element(By.CSS_SELECTOR, ".name")
```

# ID 选择器

使用标签中唯一的 `id='name'` 属性进行元素定位。

在使用时，通过井号 `#` 加 ID 名进行标识。

**CSS 代码**

```css
#name {
    color: orange;
}
```

**Selenium 代码**

```python
element = driver.find_element(By.CSS_SELECTOR, "#name")
```

# 父子选择器

又称派生选择器，根据 HTML 元素包含关系来对内层元素进行选择。

在使用时，使用空格按层级将不同选择器组合使用。

比如要选中下面 div 标签内的 span，而不是另外一个 span 时，代码可以是：

```html
<div id='mydiv'>
    <p>
        <span>123</span>
    </p>
</div>
<span>123</span>
```

**CSS 代码**

```css
div p span {
    color: orange;
}
```

**Selenium 代码**

```python
element = driver.find_element(By.CSS_SELECTOR, "div p span")
```

在实际使用时，父子选择器可以跨层级使用，比如直接 `div span` 跨过 p 标签，也可以结合类选择器和 ID 选择器一起使用，比如 `#mydiv p span` 使用 ID 选择器指定第一个元素，再按层次写入。

# 直接子元选择器

与父子选择器类似，但直接子元选择器只能选择最近一级的元素，禁止跨层。

在使用时，使用尖括号 `>` 按层级逐个将不同选择器组合使用。

比如要选中下面 div 标签内 strong 标签里的 em 元素，代码可以是：

```html
<div>
    <p>123</p>
    <strong>
        <em>234</em>
    </strong>
</div>
```

**CSS 代码**

```css
div > strong > em {
    color: orange;
}
```

**Selenium 代码**

```python
element = driver.find_element(By.CSS_SELECTOR, "div > strong > em")
```

再次提醒，直接子元选择器必须逐层使用，禁止跨层，比如 `div > em`  是无法定位的。

# 其他选择器

下面的这些选择器大部分情况下只适用于 CSS 文件，在 Selenium 使用时，会有概率无法使用，请自行识别。

**链接伪类选择器**

- `a:link`：定位一个未点击过的超链接 a 标签
- `a:visited`：定位一个访问过的超链接 a 标签
- `a:hover`：定位一个鼠标指针悬停时的超链接 a 标签
- `a:active`：定位一个鼠标点击但是没松手（激活）时的超链接 a 标签

**结构伪类选择器**

- `selector:first-child`：定位一组兄弟元素中的第一个元素
- `selector:last-child`：定位一组兄弟元素中的最后一个元素
- `selector:nth-child(n)`：定位一组兄弟元素中的第 n 个元素
- `selector:nth-last-child(n)`：定位一组兄弟元素中倒序的第 n 个元素
- `selector:first-of-type`：定位一组同类型兄弟元素中的第一个元素
- `selector:last-of-type`：定位一组同类型兄弟元素中的最后一个元素
- `selector:nth-of-type(n)`：定位一组同类型兄弟元素中的第 n 个元素
- `selector:nth-last-of-type(n)`：定位一组同类型兄弟元素中倒序的第 n 个元素

**否定伪类选择器**

- `selector:not(selector)`：在选择时，除了某一个标签外的其他元素都选择

**通配符选择器**

- `*`：定位选取所有元素
