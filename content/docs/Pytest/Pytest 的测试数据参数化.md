---
title: Pytest 的测试数据参数化
date: 2024-12-25T16:08:00+08:00
author: LiangMingJian
---

# 需求

测试数据参数化，可以自动在同一个测试中替换不同参数进行测试。

# 使用 fixtrue.params 实现

```python
data = ['anjing', 'test', 'admin']

@pytest.fixture(params=data)
def login(request):
    print('登录功能')
    yield request.param
    print('退出登录')
```

# 使用`@pytest.mark.parametrize()`实现

```python
def parametrize(self,argnames,argvalues,indirect=False,ids=None,scope=None):
    """
    argnames: 参数名称，字符串，多个使用逗号隔开
    argvalues：值，必须是列表
    indirect：是否作为函数执行
    """
    pass

@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    print(f"测试数据{test_input},期望结果{expected}")
    assert eval(test_input) == expected
```

# 对类的数据参数化

当装饰器`@pytest.mark.parametrize`装饰测试类时，会将数据集合传递给类的所有测试用例方法。注意，此时所有的测试方法都必须传入这些参数。

```python
@pytest.mark.parametrize('a, b, expect', data_1)
class TestParametrize:

    def test_parametrize_1(self, a, b, expect):
        print('\n测试函数11111 测试数据为\n{}-{}'.format(a, b))
        assert a + b == expect

    def test_parametrize_2(self, a, b, expect):
        print('\n测试函数22222 测试数据为\n{}-{}'.format(a, b))
        assert a + b == expect
```

# 多个数据的参数化

一个函数或一个类可以装饰多个 `@pytest.mark.parametrize` ，这种方式，最终生成的用例数是`n*m`。

```python
data_1 = [1, 2, 3]
data_2 = ['a', 'b']

@pytest.mark.parametrize('a', data_1)
@pytest.mark.parametrize('b', data_2)
def test_parametrize_1(a, b):
    print(f'笛卡尔积 测试数据为 ： {a}, {b}')
```

{{< details "参考文件" >}} 
1：[ 参数化@pytest.mark.parametrize @小菠萝测试笔记  ](https://www.cnblogs.com/poloyy/p/12675457.html)
{{< /details >}}

# 拓展阅读：使用 DDT 进行数据驱动

DDT（Data Driven Testing），数据驱动，简单来说就是测试数据的参数化，在 Python 中，DDT 以装饰器的形式，结合单元测试一起来使用，用来装饰测试类，为测试用例传递参数。

```python
pip install ddt
```

**基本使用**

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

**支持的装饰器**

- `@ddt.data()`：包含多个你想要传给测试用例的参数，适用动态参数，把传进来的数组组成元组，再对元组进行用例的遍历，根据索引取值相当于对每个参数进行遍历。
- `@ddt.file_data()`：会从 json 或 yaml 中加载数据，遍历加载的数据。

**支持的方法**

`@ddt.unpack`：通常 data 中包含的每一个值都会作为一个单独的参数传给测试方法，如果这些值是用元组或者列表传进来的，那么可以用 unpack 方法将其自动分解成多个参数。

```python
@data(a,b)         
# a 和 b 各运行一次用例
@data([a,d],[c,d])
# 如果没有 unpack，那么 [a,b] 当成一个参数传入用例运行
# 如果有 unpack，那么 [a,b] 被分解开，按照用例中的两个参数传递
```

**支持的类装饰器** 

`@ddt.ddt`：告知解释器 dtt 数据驱动将对类使用
