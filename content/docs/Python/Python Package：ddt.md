---
title: Python Package：ddt
date: 2024-12-25T16:11:27+08:00
author: LiangMingJian
---

# 概述

DDT(Data Driven Testing)，数据驱动，简单来说就是测试数据的参数化，在 Python 中，DDT 以装饰器的形式，结合单元测试一起来使用，用来装饰测试类，为测试用例传递参数。

```python
pip install ddt
```

# 使用

```python
import unittest
import ddt
 
data=[[1,2],[3,4],[5,6]]

@ddt.ddt
class MyTestCase(unittest.TestCase):
    # 只有一个参数时
    @ddt.data(1,2,3)
    def test_01(self,a):
        print(a)
    
    # 表示可变参数取值为 data([1,2],[3,4],[5,6])，若传参是 data,则后面的取值 data([[1,2],[3,4],[5,6]])
    @ddt.data(*data)
    @ddt.unpack
    def test_02(self,a,b):
        print(a,'----',b)
    
    # 和上面的相似，这里未使用变量
    @ddt.data([1,2],[3,4])  
    @ddt.unpack
    def test_03(self,a,b):
        print(a, '----', b)
```

# 支持的装饰器

- `@ddt.data()`：包含多个你想要传给测试用例的参数，适用动态参数，把传进来的数组组成元组，再对元组进行用例的遍历，根据索引取值相当于对每个参数进行遍历。
- `@ddt.file_data()`：会从 json 或 yaml 中加载数据，遍历加载的数据。

# 支持的方法

`@ddt.unpack`：通常 data 中包含的每一个值都会作为一个单独的参数传给测试方法，如果这些值是用元组或者列表传进来的，那么可以用 unpack 方法将其自动分解成多个参数。

```python
@data(a,b)         
# a 和 b 各运行一次用例
@data([a,d],[c,d])
# 如果没有 unpack，那么 [a,b] 当成一个参数传入用例运行
# 如果有 unpack，那么 [a,b] 被分解开，按照用例中的两个参数传递
```

# 支持的类装饰器 

`@ddt.ddt`：告知解释器 dtt 数据驱动将对类使用
