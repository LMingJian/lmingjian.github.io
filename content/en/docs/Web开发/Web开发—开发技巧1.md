---
title: 网页开发技巧1
date: 2021-09-17
author: LM
---

## 1.默认参数

在 ES6 后，用户可以在创建函数时设置默认参数。

```javascript
function fn (name = 'ABC', age = 18) {
  console.log(name, age)
}
fn() // ABC 18
fn('ABCD', 22) // ABCD 22
```

## 2.let  和 const

- `var`：有变量提升，值可变，允许重复声明
- `let`：没有变量提升，值可变，不允许重复声明
- `const`：没有变量提升，值不可变，但如果是定义对象，则属性可变

```javascript
// let 无变量提升
t = 2;
console.log(t);
let t;
// Cannot access 't' before initialization
console.log(t);
let t;
// t is not defined
t1 = 2;
console.log(t1);
var t1;
// 2
console.log(t1);
var t1;
// undefined
```

PS：变量提升(声明提升)：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。

```javascript
x = 5; // 变量 x 设置为 5
console.log(x);
var x; // 声明 x
```

实际上，上述代码等于下述代码，在运行时，x 的的声明被提升到最顶部

```javascript
var x; // 声明 x
x = 5; // 变量 x 设置为 5
console.log(x);
```

需要注意的是，JavaScript 只有声明的变量会提升，初始化的不会。如下述实例中的 y 便输出了 undefined，这是因为变量声明 var y 提升了，但是其初始化 y = 7并不会提升，所以 y 是一个未定义的变量。

```javascript
console.log(y); // undefined
var y = 7; // 初始化 y
```

上述代码等于

```javascript
var y;
console.log(y);
y = 7;
```

let 解决了诸如下述代码的问题

```javascript
for(var i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  })
} // 5 5 5 5 5 

for(let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i)
  })
} // 0 1 2 3 4
```

setTimeout 的使用，使得 i 的输出延迟，又因为 var 是函数作用域，会变量提升，所以上述 var 的代码类似于

```javascript
var i
for(i = 0; i < 5; i++) {
  setTimeout(() => {})
} 
console.log(i) // 5 5 5 5 5 
```

而由于 let 是块作用域，不会变量提升，所谓 let 的值仅作用于使用中括号`{}`包含的区域，如`for，while，if`语句，因此不会出现如 var 那样的问题

## 3.扩展运算符： ... 

对象中的扩展运算符`...`用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中

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

## 4.剩余参数

剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

```javascript
function fn (name, ...params) {
  console.log(name)
  console.log(params)
}
fn ('ABCD', 1, 2) // ABCD [ 1, 2 ]
fn ('ABCD', 1, 2, 3, 4, 5) // ABCD [ 1, 2, 3, 4, 5 ]
```

## 5.模板字符串

ES6 支持用户使用反引号创建字符串，所创建的字符串会使用`${}`进行格式化输入

```javascript
const name = 'ABCD'
const age = '22'

console.log(`${name}今年${age}岁啦`) // ABCD今年22岁啦
```

## 7.箭头函数

```javascript
const fn = () => {}

// 如果只有一个参数，可以省略括号
const fn = name => {}

// 如果函数体里只有一句return
const fn = name => {
    return 2 * name
}
// 可简写为
const fn = name => 2 * name
// 如果返回的是对象
const fn = name => ({ name: name })
```

普通函数和箭头函数的区别：

- 箭头函数不可作为构造函数，不能使用 new
- 箭头函数没有自己的 this，箭头函数里的 this 执行函数外
- 箭头函数没有 arguments 对象
- 箭头函数没有原型对象

## 8.数组遍历

```javascript
const eachArr = [1, 2, 3, 4, 5]

// 三个参数：遍历项 索引 数组本身
// 配合箭头函数
eachArr.forEach((item, index, arr) => {
  console.log(item, index, arr)
})
1 0 [ 1, 2, 3, 4, 5 ]
2 1 [ 1, 2, 3, 4, 5 ]
3 2 [ 1, 2, 3, 4, 5 ]
4 3 [ 1, 2, 3, 4, 5 ]
5 4 [ 1, 2, 3, 4, 5 ]
```

## 9.数组内容处理

```javascript
const mapArr = [1, 2, 3, 4, 5]

// 三个参数：遍历项 索引 数组本身
// 配合箭头函数，对每一个元素进行翻倍
const mapArr2 = mapArr.map((num, index, arr) => 2 * num)
console.log(mapArr2)
[ 2, 4, 6, 8, 10 ]
```

## 10.数组过滤

```js
const filterArr = [1, 2, 3, 4, 5]

// 三个参数：遍历项 索引 数组本身
// 配合箭头函数，返回大于3的集合
const filterArr2 = filterArr.filter((num, index, arr) => num > 3)
console.log(filterArr2)
[ 4, 5 ]
```

## 11.find 和 findIndex

- find：从数组中找到返回被找元素，找不到返回 undefined
- findIndex：从数组中找到返回被找元素索引，找不到返回 -1

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
```

## 12.for of  和  for  in

- for in ：以任意顺序遍历一个对象的除 Symbol 以外的可枚举属性。
- for of ：在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

无论是 for in 还是 for of 语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

- for in 语句以任意顺序迭代对象的可枚举属性。
- for of 语句遍历可迭代对象定义要迭代的数据。

我们来看以下示例

```javascript
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';
console.log(iterable)
// [3, 5, 7, foo: 'hello']

for (let i in iterable) {
  console.log(i); 
  // 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); 
    // 0, 1, 2, "foo"
  }
}

for (let i of iterable) {
  console.log(i); 
  // 3, 5, 7
}
```

上述代码中每个对象 Object 都将继承 objCustom 的属性，并且作为 Array 的每个对象要继承 arrCustom 属性，因此对象 iterable 继承属性 objCustom 和 arrCustom。

```javascript
for (let i in iterable) {
  console.log(i); // 0, 1, 2, "foo", "arrCustom", "objCustom"
}
```

上述循环打印的是 iterable 对象的所有可枚举属性，它不记录数组元素 3， 5，7 或 hello，因为这些不是枚举属性。它打印了数组的索引以及 arrCustom 和 objCustom。

```javascript
for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // 0, 1, 2, "foo"
  }
}
```

这个循环类似于第一个，但是它使用 hasOwnProperty() 来检查，检查找到的枚举属性是否是对象自己的（不是继承的）。如果是，该属性被记录。记录的属性是0，1，2和 foo，因为它们是自身的属性（不是继承的）。属性 arrCustom 和 objCustom 不会被记录，因为它们是继承的。

```javascript
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```

该循环迭代并记录 iterable 作为可迭代对象定义（ iterable，定义时是数组 ）的迭代值，即数组元素 3，5，7。

## 13.Set 和 Map

Set 对象是值的集合，你可以按照插入的顺序迭代它的元素。 Set 中的元素只会出现一次，即 Set 中的元素是唯一的，不重复的。

```js
// 可不传数组
const set1 = new Set()
set1.add(1)
set1.add(2)
console.log(set1) // Set(2) { 1, 2 }
// 也可传数组
const set2 = new Set([1, 2, 3])
// 增加元素 使用 add
set2.add(4)
set2.add('A')
console.log(set2) // Set(5) { 1, 2, 3, 4, 'A' }
// 是否含有某个元素 使用 has
console.log(set2.has(2)) // true
// 查看长度 使用 size
console.log(set2.size) // 5
// 删除元素 使用 delete
set2.delete(2)
console.log(set2) // Set(4) { 1, 3, 4, 'A' }
// 增加一个已有元素，则增加无效，会被自动去重
const set1 = new Set([1])
set1.add(1)
console.log(set1) // Set(1) { 1 }
// 传入的数组中有重复项，会自动去重
const set2 = new Set([1, 2, 'A', 3, 3, 'A'])
console.log(set2) // Set(4) { 1, 2, 'A', 3 }
// 两个对象都是不同的指针，所以没法去重
const set1 = new Set([1, {name: 'A'}, 2, {name: 'A'}])
console.log(set1) // Set(4) { 1, { name: 'A' }, 2, { name: 'A' } }
// 如果是两个对象是同一指针，则能去重
const obj = {name: 'A'}
const set2 = new Set([1, obj, 2, obj])
console.log(set2) // Set(3) { 1, { name: 'A' }, 2 }
// NaN !== NaN，NaN是自身不等于自身的，但是在Set中他还是会被去重
const set = new Set([1, NaN, 1, NaN])
console.log(set) // Set(2) { 1, NaN }
// Set可利用扩展运算符转为数组，以实现数组去重
const arr = [1, 2, 3, 4, 4, 5, 5, 66, 9, 1]
const quchongArr = [...new Set(arr)]
console.log(quchongArr) // [1,  2, 3, 4, 5, 66, 9]
```

Map 对象保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者原始值）都可以作为一个键或一个值。Map 对比 object 最大的好处就是，key 不受类型限制

```javascript
const map1 = new Map()
// 新增键值对 使用 set(key, value)
map1.set(true, 1)
map1.set(1, 2)
map1.set('哈哈', '嘻嘻嘻')
console.log(map1) // Map(3) { true => 1, 1 => 2, '哈哈' => '嘻嘻嘻' }
// 判断map是否含有某个key 使用 has(key)
console.log(map1.has('哈哈')) // true
// 获取map中某个key对应的value 使用 get(key)
console.log(map1.get(true)) // 2
// 删除map中某个键值对 使用 delete(key)
map1.delete('哈哈')
console.log(map1) // Map(2) { true => 1, 1 => 2 }
// 定义map，也可传入键值对数组集合
const map2 = new Map([[true, 1], [1, 2], ['哈哈', '嘻嘻嘻']])
console.log(map2) // Map(3) { true => 1, 1 => 2, '哈哈' => '嘻嘻嘻' }
```

## 14.includes

传入元素，如果数组中能找到此元素，则返回 true，否则返回 false

```javascript
const includeArr = [1, 2 , 3, 'A', 'A']

const isKobe = includeArr.includes('A')
console.log(isKobe) // true
```

跟 indexOf 很像，但还是有区别的

```javascript
const arr = [1, 2, NaN]
console.log(arr.indexOf(NaN)) // -1  indexOf找不到NaN
console.log(arr.includes(NaN)) // true includes能找到NaN
```

## 15.求幂运算符

ES7 提供了求幂运算符：`**`

```javascript
const num = 3 ** 2 // 9
```

## 16.获取对象的键值对集合

```javascript
const obj = {
  name: 'A',
  age: 22,
  gender: '男'
}

const entries = Object.entries(obj)
console.log(entries) 
// [ [ 'name', 'A' ], [ 'age', 22 ], [ 'gender', '男' ] ]
```

## 17.对象转成键值对数组

ES8 的 Object.entries 是把对象转成键值对数组，而 Object.fromEntries 则相反，是把键值对数组转为对象

```javascript
const arr = [
  ['name', 'A'],
  ['age', 22],
  ['gender', '男']
]

console.log(Object.fromEntries(arr)) // { name: 'A', age: 22, gender: '男' }
```

此外还可以把 Map 转换为对象

```javascript
const map = new Map()
map.set('name', 'A')
map.set('age', 22)
map.set('gender', '男')

console.log(map) // Map(3) { 'name' => 'A', 'age' => 22, 'gender' => '男' }

const obj = Object.fromEntries(map)
console.log(obj) // { name: 'A', age: 22, gender: '男' }
```

## 18.清除字符串首尾的空格

trim 方法，可以清除字符串首尾的空格

```js
const str = '    A    '
console.log(str.trim()) // 'A'
```

trimStart 和trimEnd 用来单独去除字符串的首和尾的空格

```js
const str = '    林三心    '

// 去除首部空格
console.log(str.trimStart()) // '林三心   '
// 去除尾部空格
console.log(str.trimEnd()) // '   林三心'
```

## 19.?? 和 ?.

`?.`中文名为可选链，可以使某些空变量，可以进行操作

比如有一个对象，我要取一个可能不存在的值，甚至我们都不确定obj是否存在

```js
const obj = {
  cat: {
    name: '哈哈'
  }
}
const dog = obj && obj.dog && obj.dog.name // undefined

// 可选链
const obj = {
  cat: {
    name: '哈哈'
  }
}
const dog = obj?.dog?.name // undefined
```

比如有一个数组，我不确定它存不存在，存在的话就取索引为1的值

```js
const arr = null
// do something
const item = arr && arr[1]

// 可选链
const arr = null
// do something
const item = arr?.[1]
```

比如有一个函数，我们不确定它存不存在，存在的话就执行它

```js
const fn = null
// do something
const res = fn && fn()

// 可选链
const fn = null
// do something
const res = fn?.()
```

`??`中文名为空位合并运算符，`??`和`||`最大的区别是，`??`只有`undefined, null`才算假值

```js
const a = 0 || 'A' // A
const b = '' || 'A' // A
const c = false || 'A' // A
const d = undefined || 'A' // A
const e = null || 'A' // A
const a = 0 ?? 'A' // 0
const b = '' ?? 'A' // ''
const c = false ?? 'A' // false
const d = undefined ?? 'A' // A
const e = null ?? 'A' // A
```

## 20.数字分隔符

数字分隔符可以让你在定义长数字时，更加地一目了然

```js
const num = 1000000000

// 使用数字分隔符
const num = 1_000_000_000
```

{{< details "参考文件" >}} 
1：[ ES6-ES12的开发技巧 @Sunshine_Lin ](https://juejin.cn/post/6995334897065787422)
{{< /details >}}