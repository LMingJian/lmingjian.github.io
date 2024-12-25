---
title: JS 的数组
date: 2024-12-25T16:16:40+08:00
author: LiangMingJian
---

# 概述

在 javascript 中，形如 `[1, 2, 3, 4, 5]`的数据称为数组。

# 数组的遍历

```javascript
const eachArr = [1, 2, 3, 4, 5]

eachArr.forEach((item, index, arr) => {
  console.log(item, index, arr)
})
// 遍历项 索引 数组本身
1 0 [ 1, 2, 3, 4, 5 ]
2 1 [ 1, 2, 3, 4, 5 ]
3 2 [ 1, 2, 3, 4, 5 ]
4 3 [ 1, 2, 3, 4, 5 ]
5 4 [ 1, 2, 3, 4, 5 ]
```

# 数组的查找

- **find**：使用 find 可以从数组中找到并返回第一个被找到元素，找不到则返回 undefined
- **findIndex**：使用 findIndex 可以从数组中找到返回被找元素索引，找不到返回 -1
- **includes**：使用 includes 可以判断被找元素是否在数组里面，找到返回 true，找不到返回 false
- **indexOf**：使用 indexOf 可以判断数组中是否存在某个值，如果存在，则返回数组元素的下标，否则返回 -1

```javascript
const findArr = [
  { name: '科比', no: '24' },
  { name: '罗斯', no: '1' },
  { name: '利拉德', no: '0' }
]

const kobe = findArr.find(({ name }) => name === '科比')
const kobeIndex = findArr.findIndex(({ name }) => name === '科比')
console.log(kobe) // { name: '科比', no: '24' }
console.log(kobeIndex) // 0

const includeArr = [1, 2 , 3, 'A', 'A']
const isKobe = includeArr.includes('A')
console.log(isKobe) // true

let arr = ['something', 'anything', 'nothing', 'anything'];
let index = arr.indexOf('nothing');
console.log(index) // 结果是 2
```

# 数组的过滤

```js
const filterArr = [1, 2, 3, 4, 5]

// 三个参数：遍历项 索引 数组本身
// 配合箭头函数，返回大于3的集合
const filterArr2 = filterArr.filter((num, index, arr) => num > 3)
console.log(filterArr2)
[ 4, 5 ]
```

# 数组的合成

使用扩展运算符`...`，可以便捷的拿出对象中的所有可遍历属性，然后拷贝到当前对象中。

```javascript
const arr1 = [1, 2, 4]
const arr2 = [4, 5, 7]
const arr3 = [7, 8, 9]

const arr = [...arr1, ...arr2, ...arr3]
[
  1, 2, 4, 4, 5,
  7, 7, 8, 9
]
const arrs = [...arr, ...[1, 2]]
```
