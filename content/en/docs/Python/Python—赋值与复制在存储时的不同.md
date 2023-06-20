---
title: Python 赋值与复制在存储时的不同
date: 2021-03-28
author: LM
---

## 1.前言

在 Python 中，对象赋值，浅复制，深复制在内存存储时存在些许不同，需要加以注意。

## 1.对象赋值

```python
will = ["Will", 28, ["Python", "C#", "JavaScript"]]
wilber = will

print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])

will[0] = "Wilber"
will[2].append("CSS")
print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])
```

![](/images/drawingbed/img/202205051038771.png)

### 代码分析

- 一个名为 will 的变量，与对应 list 对象的物理地址如下图所示
- 然后，使用等号将 will 变量对 wilber 变量进行赋值，那么 wilber 变量将指向 will 变量对应的对象（内存地址），如下图中第二部分所示。可以这么理解，在 Python 中，对象的赋值都是进行对象引用（内存地址）传递
- 第三张图中，由于 will 和 wilber 指向同一个对象，所以对 will 的任何修改都会体现在 wilber 上，这里需要注意的一点是，str 是不可变类型，所以当修改的时候会替换旧的对象，产生一个新的地址 39758496

![](/images/drawingbed/img/202205051039148.png)

## 2.浅拷贝

```python
import copy

will = ["Will", 28, ["Python", "C#", "JavaScript"]]
wilber = copy.copy(will)

print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])

will[0] = "Wilber"
will[2].append("CSS")
print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])
```

![](/images/drawingbed/img/202205051039522.png)

### 代码分析

- 同样使用一个 will 变量，指向一个 list 类型的对象
- 然后，通过 copy 模块里面的浅拷贝函数 `copy()`，对 will 指向的对象进行浅拷贝，将生成的新对象赋值给 wilber。浅拷贝会创建一个新的对象，但是，对于对象中的元素，浅拷贝就只会使用原始元素的引用（内存地址）
- 当对 will 进行修改的时候，由于 list 的第一个元素是不可变类型字符串，所以 will 对应的 list 的第一个元素会使用一个新的对象 39758496，但是 list 的第三个元素是一个可变类型，修改操作不会产生新的对象，所以 will 的修改结果会相应的反应到 wilber 上

![](/images/drawingbed/img/202205051039851.png)

### 浅拷贝的操作

- 使用切片`[:]`操作
- 使用工厂函数（ `list / dir / set` ）
- 使用`copy()`函数

## 3.深拷贝

```python
import copy
will = ["Will", 28, ["Python", "C#", "JavaScript"]]
wilber = copy.deepcopy(will)

print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])

will[0] = "Wilber"
will[2].append("CSS")
print(id(will))
print(will)
print([id(ele) for ele in will])
print(id(wilber))
print(wilber)
print([id(ele) for ele in wilber])
```

![](/images/drawingbed/img/202205051039363.png)

### 代码分析

- 同样使用一个 will 变量，指向一个 list 类型的对象
- 然后，通过 copy 模块里面的深拷贝函数`deepcopy()`，对 will 指向的对象进行深拷贝，将生成的新对象赋值给 wilber。
- 跟浅拷贝类似，深拷贝也会创建一个新的对象，但是，**对于对象中的元素，深拷贝都会重新生成一份（有特殊情况，下面会说明），而不是简单的使用原始元素的引用（内存地址）**，例子中 will 的第三个元素指向 39737304，而 wilber 的第三个元素是一个全新的对象 39773088
- 当对 will 进行修改的时候，由于 list 的第一个元素是不可变类型，所以 will 对应的 list 的第一个元素会使用一个新的对象 39758496，但是 list 的第三个元素是一个可变类型，修改操作不会产生新的对象。但是由于 will 与 wilber 是两个不同的对象，所以 will 的修改不会影响 wilber。
- 对于非容器类型（如数字、字符串、和其他'原子'类型的对象）没有拷贝这一说，也就是说，拷贝针对的是列表，字典，元组这些内容。
- 如果元组变量中只包含原子类型对象，则不能深拷贝

![](/images/drawingbed/img/202205051039244.png)

{{< details "参考文件" >}} 
1：[ 图解Python深拷贝和浅拷贝  @田小计划  ](https://www.cnblogs.com/wilber2013/p/4645353.html)
{{< /details >}}