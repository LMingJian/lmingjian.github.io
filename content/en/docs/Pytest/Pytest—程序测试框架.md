---
title: Pytest 程序测试框架
date: 2021-01-14
author: LM
---

## 1.简介

`pytest` 是一个使构建程序测试变得容易的框架。`pytest` 构建测试具有表达性和可读性，不需要样板代码，可以在几分钟内开始对应用程序或库进行小的单元测试或复杂的功能测试。

```bash
pip install -U pytest
```

## 2.使用：执行测试

```python
python3 -m pytest --help
pytest.main(['-s'])
pytest.main(['-x', 'mytestdir'])
# [] 内传入配置参数，相对于执行 pytest -s 
```

## 3.使用：测试文件的自动收集

```python
文件名以 test_*.py 文件和 *_test.py
以 test_ 开头的函数
以 Test 开头的类, 并且不能带有 init 方法
以 test_ 开头的方法
所有的包 pakege 必须要有 __init__.py 文件
```

## 4.使用：命令行参数

```python
# 查看 pytest 版本
pytest --version
# 显示可用的内置函数参数
pytest --fixtures
# 通过命令行查看帮助信息及配置文件选项
pytest --help
# 在第N个用例失败后，结束测试执行
pytest -x              # 第01次失败，就停止测试
pytest --maxfail=2     # 出现2个失败就终止测试
# 指定测试目录
pytest testing/
# 通过关键字表达式过滤执行
# 这条命令会匹配文件名、类名、方法名匹配表达式的用例，这里这条命令会运行 TestMyClass.test_something， 不会执行 TestMyClass.test_method_simple
pytest -k "MyClass and not method"
# 运行模块中的指定用例
pytest test_mod.py::test_func
# 运行模块中的指定方法
pytest test_mod.py::TestClass::test_method
# 通过标记表达式执行
# 这条命令会执行被装饰器 @pytest.mark.slow 装饰的所有测试用例
pytest -m slow
# 获取最慢的10个用例的执行耗时
pytest --durations=10
# 在指定目录DIR生成allure报告与如果目录存在则清除目录
pytest --alluredir=DIR --clean-alluredir
```

## 5.使用：多进程执行

当cases量很多时，运行时间也会变的很长，如果想缩短脚本运行的时长，就可以用多进程来运行。使用多线程需要安装pytest-xdist。

```python
pip install -U pytest-xdist
pytest test_se.py -n NUM # NUM填写并发的进程数。
```

## 6.使用：重试

在做接口测试时，有事会遇到503或短时的网络波动，导致case运行失败，而这并非是我们期望的结果，此时可以就可以通过重试运行cases的方式来解决。使用重试需要安装pytest-rerunfailures。

```python
pip install -U pytest-rerunfailures
pytest test_se.py --reruns NUM # NUM填写重试的次数。
```

## 7.使用：显示 print 内容

在运行测试脚本时，为了调试或打印一些内容，我们会在代码中加一些print内容，但是在运行pytest时，这些内容不会显示出来。如果带上-s，就可以显示了。

```python
pytest test_se.py -s
```

## 8.使用：配置文件

pytest 的配置文件通常放在测试目录下，名称为 **pytest.ini**，命令行运行时会使用该配置文件中的配置.

```ini
#配置pytest命令行运行参数
[pytest]
addopts = -s ... 
# 空格分隔，可添加多个命令行参数 -所有参数均为插件包的参数配置测试搜索的路径
testpaths = ./scripts  
# 测试文件路径，当前目录下的scripts文件夹 -可自定义
python_files = test_* *_test check_* 
# 测试文件，当前目录下的scripts文件夹下，以test开头，以.py结尾的所有文件 -可自定义
python_classes = Test_* Test* *Suite
# 配置测试搜索的测试类名，当前目录下的scripts文件夹下，以test开头，以.py结尾的所有文件中，以Test开头的类 -可自定义
python_functions = test_* *_test check_*
# 配置测试搜索的测试函数名，当前目录下的scripts文件夹下，以test开头，以.py结尾的所有文件中，以Test开头的类内，以test_开头的方法
```

## 9.使用：跳过测试

```python
# 根据特定的条件，不执行标识的测试函数.
# 方法：
     skipif(condition, reason=None)
# 参数：
     condition: 跳过的条件, 必传参数
     reason: 标注原因, 必传参数
# 使用方法：
     @pytest.mark.skipif(condition, reason="xxx") 
# 使用----------------------------------------------------------------------
class Test_ABC:    
    def test_a(self):
        assert 1
    @pytest.mark.skipif(condition=2>1,reason = "跳过该函数") # 跳过测试函数test_b
    def test_b(self):
        assert 0
    def test_c(self):
        pytest.skip()
```

## 10.使用：标记为预期失败

```python
# 标记测试函数为失败函数
# 方法：
     xfail(condition=None, reason=None, raises=None, run=True, strict=False)
# 常用参数：
     condition: 预期失败的条件, 必传参数
     reason: 失败的原因, 必传参数
# 使用方法:
     @pytest.mark.xfail(condition, reason="xx")
# 使用----------------------------------------------------------------------
class Test_ABC:
    def test_a(self):
        assert 1
    @pytest.mark.xfail(2 > 1, reason="标注为预期失败") # 标记为预期失败函数test_b
    def test_b(self):
        assert 0
    def test_c(self):
        pytest.xfail()
```

## 11.使用：数据参数化

```python
# 方便测试函数对测试属于的获取。
# 方法：
     parametrize(argnames, argvalues, indirect=False, ids=None, scope=None)
# 常用参数：
     argnames: 参数名
     argvalues: 参数对应值, 类型必须为list
           当参数为一个时格式: [value]
           当参数个数大于一个时, 格式为: [(param_value1,param_value2.....),(param_value1,param_value2.....)]
# 使用方法:
     @pytest.mark.parametrize(argnames,argvalues)
# 使用----------------------------------------------------------------------
@pytest.mark.parametrize("a",[3,6]) # a参数被赋予两个值，函数会运行两遍
@pytest.mark.parametrize("a,b",[(1,2),(0,3)]) # 参数a,b均被赋予两个值，函数会运行两遍
def return_test_data():
    return [(1,2),(0,3)]
@pytest.mark.parametrize("a,b",return_test_data()) # 使用函数返回值的形式传入参数值
```

## 12.使用：只执行标记用例

```bash
@pytest.mark.webtest
# 标记测试函数为 webtest 执行时可进行选择
pytest -v -m webtest
pytest -v -m 'not webtest'
```