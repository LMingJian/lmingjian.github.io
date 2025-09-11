---
title: Python 的字典
date: 2024-12-25T16:10:33+08:00
author: LiangMingJian
---

# 前言

Python 通过 class dict 实现映射关系，有且只有这一种标准映射类型。

除了列表，字典或其他可变类型，字典 dict 的键几乎可以是任何值。

# 字典的构建

**通过花括号构建**

```python
map1 = {}
map2 = {'user': 'Python', 'age': 24}
map3 = {x: x for x in range(10)}
# map3 = {0: 0, 1: 1, 2: 2, 3: 3, 4: 4, 5: 5, 6: 6, 7: 7, 8: 8, 9: 9}
```

**通过类型构造器构建**

```python
map1 = dict()
map2 = dict(user='Python', age=24)
map3 = dict([('user', 'Python'), ('age', 24)])
```

# 字典的查找

`d[key]`：**返回键值。**

```python
map1 = {'user': 'Python', 'age': 24}

print(map1['user'])  # 输出：Python
```

`key in d 或 key not in d`：**判断字典是否存在某一个键。**

```python
map1 = {'user': 'Python', 'age': 24}

print('user' in map1)  # True
print('abc' in map1)  # False
```

`d.get(key, default=None)`：**返回字典中键为 key 的值，如果没有这个键值，则返回 default 所设置的内容。**

> 与 `d[key]` 不同，没有键值的时候，`d.get()` 不会抛出异常。

```python
map1 = {'user': 'Python', 'age': 24}  
  
print(map1.get('user'))  # 输出：Python
print(map1.get('abc'))  # 输出：None
```

`d.items()`：**返回一个由键值对 `(key, value)` 组成的字典视图。**

> 字典视图是一个动态的视图对象，当字典变更时，字典视图也随之变化，字典视图支持通过迭代来处理字典数据，对字典成员进行处理。

```python
map1 = {'user': 'Python', 'age': 24}  
  
for key, value in map1.items():
    print(f'{key}: {value}')  # 输出：user: Python 和 age: 24
```

`d.keys()`：**返回一个由所有键组成的字典视图。**

```python
map1 = {'user': 'Python', 'age': 24}  
  
for key in map1.keys():
    print(f'{key}')  # 输出：user 和 age
```

`d.values()`：**返回一个由所有值组成的字典视图。**

```python
map1 = {'user': 'Python', 'age': 24}  
  
for value in map1.values():
    print(f'{value}')  # 输出：Python 和 24
```

`list(d)`：**返回一个由所有键组成的列表。**

```python
map1 = {'user': 'Python', 'age': 24}  
  
print(list(map1))  # 输出： ['user', 'age']
```

`len(d)`：**返回字典的长度。**

```python
map1 = {'user': 'Python', 'age': 24}  
  
print(len(map1))  # 输出： 2
```

# 字典的增改

**`d[key] = value`：如果键值不存在，则新增键值，如果存在则修改键值**

```python
map1 = {'user': 'Python', 'age': 24}
map1['user'] = 'ABC'
map1['number'] = '00001'

print(map1['user'])  # 输出：ABC
print(map1['number'])  # 输出：00001
```

`d.setdefault(key, default=None)`：**如果字典存在键 key，则返回对应的值，如果不存在，则插入键为 key，值为 default 的数据。**

```python
map1 = {'user': 'Python', 'age': 24}
map1.setdefault('user')
map1.setdefault('number')

print(map1['user'])  # 输出：Python
print(map1['number'])  # 输出：None
```

`d.update(**kwargs | mapping | iterable)`：**允许通过传入关键字参数，字典，一个包含键值对 `(key, value)` 的可迭代对象（如列表）来更新字典数据。**

```python
map1 = {'user': 'Python', 'age': 24}  
map1.update(user='abc')  
map1.update([('number', '00001'), ('area', 'school')])  
map1.update({'type': 0})  
  
print(map1)
# 输出：{'user': 'abc', 'age': 24, 
#       'number': '00001', 'area': 'school',
#       'type': 0}
```

`d = d | other`：**合并 d 和 other 两者的键值，两者必须是字典，当 d 和 other 存在相同键时，以 other 的数据为准。**

> Python 版本 3.9 后才支持。

```python
map1 = {'user': 'Python', 'age': 24}  
map1 = map1 | {'number': '00001'}  
  
print(map1)
# 输出：{'user': 'Python', 'age': 24, 'number': '00001'}
```

`d |= other`：**合并 d 和 other 两者的键值，other 可以是字典，也可以是包含键值对 `(key, value)` 的可迭代对象（如列表），当 d 和 other 存在相同键时，以 other 的数据为准。**

>  Python 版本 3.9 后才支持。

```python
map1 = {'user': 'Python', 'age': 24}  
map1 |= [('number', '00001'), ('type', 0)]  
  
print(map1)
# 输出：{'user': 'Python', 'age': 24, 'number': '00001', 'type': 0}
```

# 字典的删除

`del d[key]`：**删除某个键值**

```python
map1 = {'user': 'Python', 'age': 24}
del map1['user']

print(map1)  # 输出：{'age': 24}
```

`d.pop(key, default)`：**返回某个键值，然后从字典移除该键值，如果键不存在，则返回 default，如果没有设置 default，则抛出异常。**

```python
map1 = {'user': 'Python', 'age': 24}  
data1 = map1.pop('user')  
data2 = map1.pop('type', 1)  
  
print(map1)  # 输出：{'age': 24}
print(data1)  # 输出：Python
print(data2)  # 输出：1
```

**`d.clear()`：清空字典**

```python
map1 = {'user': 'Python', 'age': 24}
map1.clear()

print(map1)  # 输出：{}
```

————————————

> [ 映射类型 dict-官方文档 ](https://docs.python.org/zh-cn/3.13/library/stdtypes.html#mapping-types-dict)
