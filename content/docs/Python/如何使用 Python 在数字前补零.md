---
title: 如何使用 Python 在数字前补零
date: 2024-12-25T16:10:18+08:00
author: LiangMingJian
---

# 需求

在随机生成的 1 位数，2 位数前补零，使得数字的位数或宽度统一，实现对数据的格式化。

# 使用`str.zfill()`方法

```python
number = 5  
formatted_number = str(number).zfill(3)  
print(formatted_number)
```

# 使用格式化字符串

```python
number = 7  
formatted_number = f"{number:03}"  
print(formatted_number)  
```

# 使用`str.format()`方法

```python
number = 10  
formatted_number = "{:03}".format(number)  
print(formatted_number)  
```

# 使用`%`运算符

```python
number = 13  
formatted_number = "%03d" % number  
print(formatted_number) 
```
