---
title: Python 数字前补零
date: 2021-09-16
author: LM
---

## 1.需求

在随机生成的 1 位数，2 位数前补零，使得数字的位数或宽度统一，实现对数据的格式化。

## 2.实现

### a.使用`str.zfill()`方法

```python
number = 5  
formatted_number = str(number).zfill(3)  
print(formatted_number)
```

### b.使用格式化字符串

```python
number = 7  
formatted_number = f"{number:03}"  
print(formatted_number)  
```

### c.使用`str.format()`方法

```python
number = 10  
formatted_number = "{:03}".format(number)  
print(formatted_number)  
```

### d.使用`%`运算符

```python
number = 13  
formatted_number = "%03d" % number  
print(formatted_number) 
```

