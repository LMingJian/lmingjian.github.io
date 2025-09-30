---
title: Python 的列表
date: 2025-09-26T09:12:15+08:00
author: LiangMingJian
---

# 前言

Python 的列表是一种序列（Sequence）结构，序列中的每个元素都会被分配一个整数来唯一确定它的位置， 这个整数就称为索引，索引总是从 0 开始，依此类推。

Python 的列表是支持修改内部元素的序列。与列表类似，但不支持修改的序列是元组，元组支持的操作与列表基本一致。

列表是以中括号 `[]` 包裹元素定义的；元组是以小括号 `()` 包裹元素定义的。

# 列表的创建

通过中括号 `[]` 包裹一定数据即可定义一个列表变量，包裹的多个数据使用逗号分隔，且数据项可以是不同的数据类型。

```python
list1 = [1, '2', [3, 4], {5: 6}]
```

特别的，支持通过**列表推导式**来快速的创建列表。

形如 `do-something for each in iteration` 格式的语句称为一个列表推导式，其能便捷的对一个迭代体里面的每一个元素单独处理。

```python
lista = [1, 2, 3]

# 列表推导式
list0 = [x * 2 for x in lista]
```

最后，还可以通过 `list()` 方法将其他数据类型，如字符串，元组，字典等转换为列表。

```python
# 将字符串转换为每个字符组成的 list
list0 = list("abcdef")

# 将元组转化为 list
list1 = list((1, 2, 3))

# 将字典转换为 list，默认转换 key 到 list，以下方式是等同的
dic0 = {"key0": "val0", "key1": "val1"}
list2 = list(dic0)
list3 = list(dic0.keys())

# 将字典的值转换为 list
list4 = list(dic0.values())
```

# 列表的运算

列表支持加法与乘法运算，分别完成列表的合并与重复操作。

```python
# 列表的加法
list0 = [0, 1, 2, 3, 4]
list1 = ['a', 'b', 'c'] + list0
print(list1)
# ['a', 'b‘, 'c', 0, 1, 2, 3, 4]

# 列表的乘法（重复）
list0 = ['*'] * 5
print(list0)
# ['*', '*', '*', '*', '*']
```

# 列表的访问

列表支持通过 `list[索引]` 的方式直接访问内部元素，索引是整数，支持正整数（正向索引，从左到右）和负整数（负向索引，从右到左）。

```python
# 正向索引
list0 = [0, 1, 2, 3, 4]
print(list0[0])
# 0

# 负向索引
list1 = [0, 1, 2, 3, 4]
print(list1[-1])
# 4
```

列表也支持通过切片 `list[start:stop:step]` 的方式来批量获取内部元素。

详细的切片节省可查阅另一篇文章：[ 如何使用 Python 进行列表切片 ](https://zhuanlan.zhihu.com/p/1941792644427654837)

```python
list0 = [1, 2, 3, 4]

print(list0[0:3])  
# [1, 2, 3]
```

**特别的，还可以使用方法 `filter()` 从列表中提取特定数据，或使用方法 `enumerate()` 同时遍历列表的索引和数据。**

```python
list0 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  
  
# 通过 filter 提取数据  
filter_obj = filter(lambda x: x % 2 == 0, list0)  
print(list(filter_obj))  
# 输出: [2, 4, 6, 8, 10]

# 通过 enumerate 枚举序号和元素
for key, value in enumerate(list0):  
    print(key, value)
# 将序号和数据依次输出
```

# 列表的统计

列表可以通过方法 `len()` 统计内部元素的数量。

```python
list0 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(len(list0))
# 10
```

通过方法 `count(元素)`，可以单独统计列表中某个元素的数量。

```python
list0 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 2]  

print(list0.count(2))
# 2
```

如果需要获取列表中不同种类元素的数量，可以将列表转换为集合自动剔除重复数据，然后再计算列表长度。

```python
list0 = [1, 1, 2, 3]  

print(len(set(list0)))
# 3
```

特别的，如果列表中元素都是整型数据，可以直接使用 `max()` 和 `min()` 方法获取列表中的最大最小值。

```python
list0 = [1, 2, 3, 4, 5, 6]  

print(max(list0))  
print(min(list0))
# 6 和 1
```

# 列表的排序

列表可以通过 `sort()` 方法将内部元素进行排序，默认正序。支持传入 `reverse=True` 来实现反向排序。

特别的，反向排序还可以直接使用 `reverse()` 方法。

**需要注意的事，上述两个排序方法都会直接修改原有列表，而不会重新生成一个列表**。

```python
list0 = [1, 3, 2, 5, 4, 6]  

# 正向排序
list0.sort()  
print(list0)
# 此时列表为 [1, 2, 3, 4, 5, 6]

# 反向排序
list0.sort(reverse=True)  
print(list0)  
# 此时列表为 [6, 5, 4, 3, 2, 1]

# 再次反向排序
list0.reverse()  
print(list0)
# 此时列表为 [1, 2, 3, 4, 5, 6]
```

# 列表的插入

列表元素一般可以通过 `append()` 方法在尾部添加新元素。

当然，也支持通过 `insert(索引, 数据)` 方法在指定的索引位置插入，此时其他元素后移。

```python
list0 = [0, 1, 2, 3]

# 在尾部添加元素
list0.append(100)
# [0, 1, 2, 3, 100]

# 在指定位置插入元素
list0.insert(1, 100)
# [0, 100, 1, 2, 3, 100]
```

特别的，如果想要一次性添加多个元素，可以使用 `extend(迭代对象)` 方法在列表的尾部一次性添加。

```python
list0 = [0, 1, 2, 3]  

# 在列表尾部插入多个元素
list0.extend([99, 100])  
print(list0)
# [0, 1, 2, 3, 99, 100]
```

# 列表元素的删除

列表元素支持通过 `del()` 方法进行删除，需要注意，该方法操作的是列表本身。

```python
list0 = [1, 2, 3, 4, 5]

# 删除元素
del(list0[0])
print(list0)
# [2, 3, 4, 5]
```

方法 `del()` 删除的元素是不返回值的，如果想要在删除时返回删除的元素，可以使用 `pop()`，在不传入索引时，默认抛出列表最后一个元素。

```python
list0 = [1, 2, 3, 4, 5]

# pop()
data1 = list0.pop()
print(data1)  # 5
print(list0)  # [1, 2, 3, 4]

# pop(0)
data2 = list0.pop(0)
print(data2)  # 1
print(list0)  # [2, 3, 4]
```

特别的，如果想要依据元素的内容删除，可以使用方法 `remove(value)`。该方法会从头开始，逐个匹配元素，当匹配到与传入 value 相同的元素时，移除，如果元素不存在，则抛出错误。

```python
list0 = [1, 2, 3, 4, 5]

# 删除 3
list0.remove(3)  
print(list0)
# [1, 2, 4, 5]

# 删除 6 
list0.remove(6)  
# ValueError: list.remove(x): x not in list
```

# 列表元素的判断

要判断一个元素是否在列表中，可以直接使用 in 和 not in 运算符。

```python
list0 = [1, 2, 3, 4, 5]

print(1 in list0)      # True
print(1 not in list0)  # False
```

另外，还可以通过 `index()` 获取一个元素在列表中的位置，如果不存在则抛出错误，通过这个方法也可以判断元素是否在列表中。

```python
list0 = [1, 2, 3, 4, 5]

print(list0.index(1))   # 0
print(list0.index(6))   # ValueError: 6 is not in list
```

# 拓展阅读：元组

元组是特别的列表，它兼容列表大部分操作，只是元组的元素是只读不可修改的。

```python
tuple0 = (1, 2, 3)  

# 获取值
print(tuple0[0])  
# 1

# 禁止修改
tuple0[0] = 100  
# TypeError: 'tuple' object does not support item assignment
```

**注意，元组如果只有一个元素，必须加上逗号，否则 Python 会将元素识别为值，而不是元组。**

```python
tuple0 = (1)  
tuple1 = (1,)  
  
print(tuple0)  # 1 整型
print(tuple1)  # (1,) 元组
```
