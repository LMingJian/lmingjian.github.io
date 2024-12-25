---
title: JS 的格式化字符串
date: 2024-12-25T16:16:32+08:00
author: LiangMingJian
---

# 格式化字符串

ES6 支持用户使用反引号创建字符串，所创建的字符串会使用`${}`进行格式化输入。

```javascript
const name = 'ABCD'
const age = '22'

console.log(`${name}今年${age}岁啦`) // ABCD今年22岁啦
```
