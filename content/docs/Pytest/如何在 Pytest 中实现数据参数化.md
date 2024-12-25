---
title: 如何在 Pytest 中实现数据参数化
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
