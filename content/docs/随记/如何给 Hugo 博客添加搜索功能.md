---
title: 如何给 Hugo 博客添加搜索功能
date: 2024-12-25T10:13:59+08:00
author: LiangMingJian
---

# 利用 Hugo 自带的 index.xml 文件

由于 Hugo 生成的 index.xml 文件包含博客所有文章的标题，链接和概要，因此可以利用这个文件来进行内容搜索。

在使用 JavaScript 进行查找时，可以使用方法 `indexOf()`，返回值在字符串中第一次出现的位置，如果没找到该值，则返回 `-1`。

```JavaScript
let text = "Hello world, welcome to the universe.";
let result = text.indexOf("welcome");
```

`indexOf()` 不支持正则输入，如果要使用正则，则建议使用方法 `search()`，同样返回匹配项的位置，如果没找到，则返回 `-1`。

```JavaScript
let text = "Mr. Blue has a blue house";
let position = text.search(/blue/);
```

# 利用 Bing 搜索引擎实现

- Bing 搜索可以收录 GitHub Page ，因此可以在 Bing 中进行搜索。
- 需要注意的是 GitHub Page 禁止了百度爬取，因此 GitHub Page 的内容不能在百度搜索。
- 使用`site:(https://your-page-name.github.io)` 可以将搜索范围固定在对应的 GitHub Page 页面中。

————————————

> [ 给 Hugo 站点添加搜索功能 - Go 语言中文网 ](https://studygolang.com/articles/27141?fr=sidebar)
