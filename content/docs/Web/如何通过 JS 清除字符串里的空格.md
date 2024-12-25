---
title: 如何通过 JS 清除字符串里的空格
date: 2024-12-25T16:15:40+08:00
author: LiangMingJian
---

# 使用 trim

`trim()` 可以清除字符串首尾的空格。

```javascript
const str = '    A    '
console.log(str.trim()) // 'A'
```

# 使用 trimStart 和 trimEnd

`trimStart()` 和 `trimEnd()` 可分别去除字符串的首和尾的空格。

```javascript
const str = '    AAA    '

// 去除首部空格
console.log(str.trimStart()) // 'AAA   '
// 去除尾部空格
console.log(str.trimEnd()) // '   AAA'
```
