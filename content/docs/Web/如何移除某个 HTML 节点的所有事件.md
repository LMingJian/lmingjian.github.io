---
title: 如何移除某个 HTML 节点的所有事件
date: 2024-12-25T16:15:49+08:00
author: LiangMingJian
---

# 克隆&替换节点

在使用`.cloneNode()`方法时，通过`.addEventListener()`附加的监听器都不会被带过去。因此，我们可以使用克隆以及替换来将某个节点下的所有事件移除。

```javascript
button.parentNode.replaceChild(button.cloneNode(true), button);
// 或者
button.replaceWith(button.cloneNode(true));
```

{{< details "参考文件" >}} 
1：[ 如何移除事件监听器 @chuck ](https://juejin.cn/post/7210756986462978108)
{{< /details >}}
