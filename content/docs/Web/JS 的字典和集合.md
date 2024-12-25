---
title: JS 的字典和集合
date: 2024-12-25T16:16:51+08:00
author: LiangMingJian
---

# 字典 Map

Map 是一组键值对的结构，任何值都可以作为一个键或一个值，具有极快的查找速度。

```javascript
// 定义 map
const map1 = new Map()

// 可直接定义 map 里面的键值对
const map2 = new Map([[true, 1], [1, 2], ['哈哈', '嘻嘻嘻']])
console.log(map2) // Map(3) { true => 1, 1 => 2, '哈哈' => '嘻嘻嘻' }

// 新增键值对
map1.set(true, 1)
map1.set(1, 2)
map1.set('哈哈', '嘻嘻嘻')
console.log(map1) // Map(3) { true => 1, 1 => 2, '哈哈' => '嘻嘻嘻' }

// 判断 map 是否含有某个键
console.log(map1.has('哈哈')) // true

// 获取 map 中某个键对应的值
console.log(map1.get(true)) // 2

// 删除 map 中某个键值对
map1.delete('哈哈')
console.log(map1) // Map(2) { true => 1, 1 => 2 }

```

# 集合 Set

Set 是值的集合，Set 允许储存任何类型的唯一值，无论是原始值或者是对象引用。需要注意的是，Set 中的元素只会出现一次，即 Set 中的元素是唯一的，不重复的。

```javascript
// 定义 set
const set1 = new Set()

// 定义 set 并直接赋值
const set2 = new Set([1, 2, 3])

// 添加元素
set1.add(1)
set1.add(2)
console.log(set1) // Set(2) { 1, 2 }

// 是否含有某个元素
console.log(set2.has(2)) // true

// 查看长度
console.log(set2.size) // 5

// 删除元素
set2.delete(2)
console.log(set2) // Set(4) { 1, 3, 4, 'A' }

// 当传入的数组中有重复项，会自动去重
const set2 = new Set([1, 2, 'A', 3, 3, 'A'])
console.log(set2) // Set(4) { 1, 2, 'A', 3 }

// 当两个对象都是不同的指针，所以没法去重
const set1 = new Set([1, {name: 'A'}, 2, {name: 'A'}])
console.log(set1) // Set(4) { 1, { name: 'A' }, 2, { name: 'A' } }

// 如果是两个对象是同一指针，则能去重
const obj = {name: 'A'}
const set2 = new Set([1, obj, 2, obj])
console.log(set2) // Set(3) { 1, { name: 'A' }, 2 }

// NaN !== NaN，NaN 自身不等于自身，但在 set 中，还是会被去重
const set = new Set([1, NaN, 1, NaN])
console.log(set) // Set(2) { 1, NaN }

// set 可利用扩展运算符转为数组，以实现数组去重
const arr = [1, 2, 3, 4, 4, 5, 5, 66, 9, 1]
const quchongArr = [...new Set(arr)]
console.log(quchongArr) // [1,  2, 3, 4, 5, 66, 9]

```
