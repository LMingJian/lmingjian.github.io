---
title: 如何将字符串转换为 HTML 节点
date: 2024-12-25T16:15:13+08:00
author: LiangMingJian
---

# 使用 innerHTML

```javascript
function createNode(txt) {
  const template = "<div class='child'>${txt}</div>";
  let tempNode = document.createElement('div');
  tempNode.innerHTML = template;
  return tempNode.firstChild;
}

const container = document.getElementById('container');
container.appendChild(createNode('hello'));
```

# 使用 DOMParser

```javascript
function createDocument(txt) {
  const template = "<div class='child'>${txt}</div>";
  let doc = new DOMParser().parseFromString(template, 'text/html');
  let div = doc.querySelector('.child');
  return div;
}
  
const container = document.getElementById('container');
container.appendChild(createDocument('hello'));
```

# 使用 DocumentFragment

```javascript
function createDocumentFragment(txt) {
  const template = "<div class='child'>${txt}</div>";
  let frag = document.createRange().createContextualFragment(template);
  return frag;
}
 
const container = document.getElementById('container');
container.appendChild(createDocumentFragment('hello'));
```
