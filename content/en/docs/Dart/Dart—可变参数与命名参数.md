---
title: 可变参数与命名参数
date: 2021-07-22
author: LMingJian
---

## 1.可变参数

`dart` 的可变参数使用 `[]` 包含，在使用函数时可按顺序直接输入值。（PS：在新版本的语言规范中，可变参数需要被赋予初值或使用声明时使用 ? 标注）

```dart
void test(String name,[int age]){
    if(age!=null){
        print("name:$name;age:$age")
    }else{
        print("name:$name")
    }
}
void main(){
    test('A');
    test('A',15);
}
```

## 2.命名参数

`dart` 的命名参数使用 `{}` 包含，在使用函数时需指定值的名字。（PS：在新版本的语言规范中，命名参数需要被赋予初值或使用声明时使用 ? 标注）

```dart
void test(String name,{int age}){
    if(age!=null){
        print("name:$name;age:$age")
    }else{
        print("name:$name")
    }
}
void main(){
    test('A');
    test('A',age: 15);
}
```

