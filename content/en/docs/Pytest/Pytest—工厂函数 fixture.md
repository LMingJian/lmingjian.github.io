---
title: Pytest 的 fixture 函数
date: 2021-01-15
author: LM
---

## 1.简介

fixture 是 pytest 提供给测试环境初始化与清理的一个函数，通过语法糖@pytest.fixture()，测试用例会在测试开始前与测试结束后自动的执行 @pytest.fixture() 标记的函数，完成测试环境的初始化与清理。

fixture 的优势有：独立的命名且能通过声明从测试函数、模块、类中使用；按模块化的方式实现，每个 fixture 都可以互相调用；fixture 跨函数 function，类 class，模块 module 或整个测试 session 使用。

## 2.fixture 的作用范围

fixture 里通过 scope 参数控制 fixture 的作用范围：session>module>class>function

- function：每一个函数或方法都会调用
- class：每一个类调用一次，一个类中可以有多个方法
- module：每一个.py文件调用一次，该文件内又有多个function和class
- session：是多个文件调用一次，可以跨.py文件调用，每个.py文件就是module

## 3.fixture 的参数

```python
fixture(scope='function', params=None, autouse=False, ids=None, name=None)
```

- scope：有四个级别参数"function"（默认），"class"，"module"，"session"
- params：一个可选的参数列表。
- autouse：如果True，则为所有测试都可以自动使用 fixture 函数。如果为False则显示需要传参来激活fixture
- ids：每个字符串id的列表，每个字符串对应于params这样他们就是测试ID的一部分。如果没有提供ID它们将从params自动生成
- name：fixture的名称。这默认为装饰函数的名称。

## 4.fixture 的调用 

### 1.函数或类里面方法直接传 fixture 的函数参数名称

```python
@pytest.fixture()
def test1():
    print('\n开始执行function')

def test_a(test1):
    print('---用例a执行---')

class TestCase:
    def test_b(self, test1):
        print('---用例b执行')
```

### 2.使用装饰器 @pytest.mark.usefixtures() 修饰需要运行的用例

```python
@pytest.fixture()
def test1():
    print('\n开始执行function')

@pytest.mark.usefixtures('test1')
def test_a():
    print('---用例a执行---')

@pytest.mark.usefixtures('test1')
class TestCase:
    def test_b(self):
        print('---用例b执行---')
    def test_c(self):
        print('---用例c执行---')

# 调用多个fixture，叠加usefixtures
@pytest.mark.usefixtures('test1')
@pytest.mark.usefixtures('test2')
def test_a():
    print('---用例a执行---')

# 如果fixture有返回值，那么usefixture就无法获取到返回值，这个是装饰器usefixture与用例直接传fixture参数的区别。
# 当fixture需要用到return出来的参数时，只能讲参数名称直接当参数传入，不需要用到return出来的参数时，两种方式都可以。
```

### 3.自动使用 autouse=True

当用例很多的时候，每次都传这个参数，会很麻烦。fixture 里面有个参数 autouse，默认是 False 没开启的，可以设置为 True 开启自动使用 fixture 功能，这样用例就不用每次都去传参了。将 autouse 设置为 True，自动调用 fixture 功能

```python
@pytest.fixture(scope='module', autouse=True)
def test1():
    print('\n开始执行module')
```

## 2.示例：基本使用

```python
fixture(scope="function", params=None, autouse=False, ids=None, name=None)
# scope：被标记方法的作用域 
"function": 默认值, 作用于每个测试方法, 每个test都运行一次
"class": 作用于整个类, 每个class的所有test只运行一次
"module": 作用于整个模块, 每个module的所有test只运行一次
"session": 作用于整个session(慎用), 每个session只运行一次
# params：(list类型)提供参数数据，供调用标记方法的函数使用
# autouse：是否自动运行,默认为False不运行,设置为True自动运行
# 使用----------------------------------------------------------------------
@pytest.fixture()
def before(self):
    pass
def test_a(self,before): # ️test_a方法传入了被fixture标识的函数，已变量的形式
    pass
# 使用----------------------------------------------------------------------
def before():
    pass
@pytest.mark.usefixtures("before")
class Test_ABC:
    def setup(self):
        pass
    def test_a(self):
        pass
# 使用----------------------------------------------------------------------
@pytest.fixture()
def need_data():
    return 2 # 返回数字2
class Test_ABC:
    def test_a(self,need_data):
        assert need_data != 3 # 拿到返回值做一次断言
```

## 3.示例：fixture 当做参数传入

定义fixture跟定义普通函数差不多，唯一区别就是在函数上加个装饰器@pytest.fixture()，fixture命名不要以test开头，跟用例区分开。fixture是有返回值得，没有返回值默认为None。用例调用fixture的返回值，直接就是把fixture的函数名称当做变量名称。

```python
@pytest.fixture()
def K():
    a = 'leo'
    return a
def test2(K):
    assert test1 == 'leo'
```

## 4.示例：使用多个 fixture

如果用例需要用到多个fixture的返回数据，fixture也可以返回一个元祖，list或字典，然后从里面取出对应数据。

```python
@pytest.fixture()
def test1():
    a = 'leo'
    b = '123456'
    print('传出a,b')
    return (a, b)

def test2(test1):
    u = test1[0]
    p = test1[1]
    assert u == 'leo'
    assert p == '123456'
    print('元祖形式正确')
```
