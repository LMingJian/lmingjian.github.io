---
title: Javascript 中箭头函数与普通函数的区别
date: 2022-01-24
author: LM
---

## 1.定义

在 Javascript 中，形如`var foo = (a, b)=>{ return a + b };`的称为箭头函数，与常用的`function fn(){}`函数不同，箭头函数允许直接将返回值赋给变量，简化代码。

```javascript
// function
function fn(a, b){
	return a + b;
}
// arrow function
var foo = (a, b)=>{ return a + b };
```

## 2.区别：This 的指向不同

使用`function`定义的函数，`this`的指向随着调用环境的变化而变化的，而箭头函数中的`this`指向是固定不变的，一直指向的是定义函数的环境。

```javascript
// 使用function定义的函数
function foo(){
	console.log(this);
}
var obj = { aa: foo };
foo(); //Window
obj.aa() //obj { aa: foo }

// 使用箭头函数定义函数
var foo = () => { console.log(this) };
var obj = { aa:foo };
foo(); //Window
obj.aa(); //Window
```

## 3.区别：构造函数的不同

仅能通过 function 方法定义构造函数。

```javascript
// 使用 function 方法定义构造函数
function Person(name, age){
	this.name = name;
	this.age = age;
}
var lenhart =  new Person(lenhart, 25);
console.log(lenhart); //{name: 'lenhart', age: 25}

// 尝试使用箭头函数
var Person = (name, age) =>{
	this.name = name;
	this.age = age;
};
var lenhart = new Person('lenhart', 25); //Uncaught TypeError: Person is not a constructor
```

## 4.区别：变量提升的不同

在`javascript`的内存机制中，函数的级别最高，因此箭头函数一定要定义于调用前。

```javascript
foo(); //123
function foo(){
	console.log('123');
}

arrowFn(); //Uncaught TypeError: arrowFn is not a function
var arrowFn = () => {
	console.log('456');
};
```

