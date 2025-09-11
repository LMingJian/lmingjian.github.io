---
title: Python 的赋值、复制存储机制
date: 2024-12-25T16:11:24+08:00
author: LiangMingJian
---

# 前言

在 Python 中，对象赋值，浅复制，深复制在内存存储时存在些许不同，如果不加以注意，在某些调用的情况下会导致程序异常。

# 对象存储机制

在进行正文的介绍前，先简单了解下 Python 的对象存储机制。

Python 的对象分为不可变对象和可变对象两种。

**不可变对象**

如`int`, `float`, `str`, `tuple`, `frozenset`, `bytes`，在生成后，其值不可变，固定占用一个内存地址，任何的修改操作其实都是新建操作。

比如 `a = 10` ，这里的 10 就占用一个内存地址，记为 0001。当我们修改 a 时，如 `a = 20`， 这里的 20 并不会覆盖 0001 这个内存地址，而是会占用一个新的地址，记为 0002，然后将这个 0002 的地址指向 a，在修改后，0001 这个地址存储的数值还是 10。

**可变对象**

如`list`, `dict`, `set`, `bytearray`, 自定义类实例，在生成后，其值可变，修改值不会影响到对象地址，只会影响对象内部元素的地址。

比如 `a = [1, 2, 3]` ，这里的 `[1, 2, 3]` 就占用一个内存地址，记为 0001，里面的元素 1，2，3 分别占用地址 1001，1002，1003。当我们修改 a 时，如 `a[0] = 4`，这里的 4 是不可变对象，新占用一个地址 1004。这新地址会替换掉 a 里面的 1001，但不会影响到 a 本身的对象地址，即新元素 `a = [4, 2, 3]` 的内存地址还是 0001，但里面元素 4，2，3 的地址会变为 1004，1002，1003。

# 对象赋值

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

![](/_images/drawingbed/img/202205051038771.png)

![](/_images/drawingbed/img/202205051039148.png)

通过方法 `id()` 可以查看上述变量在内存中的地址。

显然，对象赋值的时候，原变量和被赋值变量所使用的内存空间是同一个，即原变量与被赋值变量都是指向同一个东西。此时修改任何一个都会修改另外一个，无论是修改数组原有元素还是给数组添加元素。

> 注意：因为字符串 str 和整型 int 是不可变对象，所以在修改时需要重新分配地址空间，而数组 list 是可变对象，所以其内存空间在修改后不会变化。

# 浅拷贝

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

![](/_images/drawingbed/img/202205051039522.png)

![](/_images/drawingbed/img/202205051039851.png)

在使用浅复制时，复制变量与原变量的内存空间不一样了，两个指向不同的数组。但对于数组里面的元素，它们的引用还是在同一个地址。

我们在上面知道修改不可变对象如字符串，整型等时，其内存空间会发生变化，在修改可变对象如列表，字典等时，对象本身的内存空间不变。

**那对于浅复制，当修改的值是不可变对象时，修改原变量不会影响到复制变量，但修改的是可变对象时，修改原变量会直接影响复制变量。**

这是因为在修改不可变对象时，元素地址发生了改变。对复制变量与原变量这两个数组来说，不会产生影响。但在修改可变对象时，元素地址不会发生改变，对复制变量与原变量这两个数组来说，就是同一个子元素在被修改，因此必然会发生影响。

**常见的浅拷贝操作：**

- 切片`[:]`
- 工厂函数（ `list / dir / set` ）
- `copy()`函数

# 深拷贝

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

![](/_images/drawingbed/img/202205051039363.png)

![](/_images/drawingbed/img/202205051039244.png)

对于深复制，与浅复制相似，同样生成两个地址的数组，但区别在于，原对象的可变对象也会被复制生成一份。

深复制生成的新对象拥有独立的内存空间，其中的可变对象也拥有独立的空间，此时，即使是在修改原变量的可变元素，都不会影响到复制变量，原变量和复制变量完全隔离。

{{< details "参考文件" >}} 

1：[ 图解Python深拷贝和浅拷贝  @田小计划  ](https://www.cnblogs.com/wilber2013/p/4645353.html)

{{< /details >}}
