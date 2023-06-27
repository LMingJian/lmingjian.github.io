---
title: 油猴 Tampermonkey
date: 2022-06-20
author: LM
---

## 1.什么是油猴( Tampermonkey )

Tampermonkey 是最为流行的用户脚本管理器扩展，它适用于 Chrome，Microsoft Edge，Firefox 等主流浏览器。配合油猴脚本( 用户脚本 / User Script )的使用，可以实现如：为网站添加新的功能；改变网站样式、排版；隐藏网站上烦人的部分内容等功能。

## 2.安装油猴( Tampermonkey )

- 前往官网获取，[ Tampermonkey ](https://www.tampermonkey.net/)
- 或在各浏览器的扩展商店中搜索 Tampermonkey

## 3.油猴脚本( User Script )

下面是一个默认的 User Script

```javascript
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://blog.csdn.net/mukes/article/details/109727662
// @icon         https://www.google.com/s2/favicons?sz=64&domain=csdn.net
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Your code here...
})();
```

### a.元数据

被 `==UserScript==`包括的数据被称为元数据，是用来向 Tampermonkey 描述这个脚本的信息。更多：[Tampermonkey • 文档](https://www.tampermonkey.net/documentation.php#metadata)

```javascript
// ==UserScript==
// @name         编写的油猴脚本名称
// @namespace    命名空间；用来区分名称相同但是作者不同的用户脚本，一般写作者的个人网址或博客地址
// @version      0.1；版本号
// @description  功能描述
// @author       作者
// @match        脚本匹配的网址，支持通配符匹配
// @include      脚本匹配的网址，支持通配符匹配，不安全，会出现警告，建议使用 match
// @exclude      排除匹配到的网址，优先于 include
// @icon         脚本的图标
// @grant        none 禁用沙盒，使脚本直接在上下文运行
// ==/UserScript==
```

### b.用户代码

函数 `(function(){ ... })() `代表立即执行

```javascript
(function() {
    'use strict';

    // Your code here...
})();
```

{{< details "参考文件" >}} 
1：[ 如何开发一个油猴脚本_ @mukes ](https://blog.csdn.net/mukes/article/details/109727662)
{{< /details >}}