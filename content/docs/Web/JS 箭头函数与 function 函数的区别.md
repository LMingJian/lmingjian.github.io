---
title: JS 箭头函数与 function 函数的区别
date: 2024-12-25T16:16:59+08:00
author: LiangMingJian
---

# 什么是箭头函数

在 Javascript 中，形如`var foo = (a, b)=>{ return a + b };`的称为箭头函数，与常用的`function fn(){}`函数不同，箭头函数允许直接将返回值赋给变量，简化代码。

```javascript
// function
function fn(a, b){
	return a + b;
}
// arrow function
var foo = (a, b)=>{ return a + b };
// 如果只有一个参数，可以省略括号
const fn = name => {}
// 如果只有一个 return，可以省略括号和 return
const fn = name => 2 * name
// 如果 return 返回的是对象
const fn = name => ({ name: name })
```

# 区别一：This 的指向不同

使用`function`定义的函数，`this`的指向随着调用环境的变化而变化的，而箭头函数中的`this`指向是固定不变的，一直指向的是定义函数的环境。

```javascript
// 使用 function 定义的函数
function foo(){
	console.log(this);
}
var obj = { aa: foo };
foo(); // Window
obj.aa() // obj { aa: foo }

// 使用箭头函数定义函数
var foo = () => { console.log(this) };
var obj = { aa:foo };
foo(); // Window
obj.aa(); // Window
```

# 区别二：构造函数的不同

仅能通过 function 方法定义构造函数。

```javascript
// 使用 function 方法定义构造函数
function Person(name, age){
	this.name = name;
	this.age = age;
}
var lenhart =  new Person(lenhart, 25);
console.log(lenhart); // {name: 'lenhart', age: 25}

// 尝试使用箭头函数
var Person = (name, age) =>{
	this.name = name;
	this.age = age;
};
var lenhart = new Person('lenhart', 25); // Uncaught TypeError: Person is not a constructor
```

# 区别三：变量提升的不同

在`javascript`的内存机制中，函数的级别最高，因此箭头函数一定要定义于调用前。

```javascript
foo(); // 123
function foo(){
	console.log('123');
}

arrowFn(); // Uncaught TypeError: arrowFn is not a function
var arrowFn = () => {
	console.log('456');
};
```
