---
title: JS 的强制类型转换
date: 2024-12-25T16:16:37+08:00
author: LiangMingJian
---

# 转换为 String

使用 `toString()` 将其他数据类型（除 option）转换成 String 类型，注意 Null 与 Undefined 无 `toString()` 方法。

```javascript
value.toString()
String(value)
```

# 转换为 Number

使用 `Number()` 将其他的数据类型转换为 Number 类型，注意对非数字字符类型会转换为 NaN，Null 会转换为 0。

```javascript
Number(value)
```

# 转换为 Boolean

使用 `Boolean()` 将其他的数据类型转换为 Boolean 类型。

```javascript
Boolean(value)
```

# 特别的，object 转字符串

```javascript
const obj = {
     id: 0,
     name: '张三',
     age: 12
}
const objToStr = JSON.stringify(obj)
console.log('obj:', obj)
console.log('objToStr:', objToStr)
```

# 特别的，json 字符串转 object

```javascript
const str = '{"id":0,"name":"张三","age":12}'
const strToObj = JSON.parse(str)
console.log('str:', str)
console.log('strToObj:', strToObj)
```
