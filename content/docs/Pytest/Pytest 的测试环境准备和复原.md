---
title: Pytest 的测试环境准备和复原
date: 2024-12-25T16:08:11+08:00
author: LiangMingJian
---

# 函数级环境

构建函数 `setup()／teardown()` ，上述两个函数在执行**测试函数**时会运行在**测试开始和测试结束**。每运行一次测试函数，就会运行一次 `setup()` 和 `teardown()`。

# 类级环境

构建函数 `setup_class() /  teardown_class()` ，上述两个函数在执行**测试类**时会运行在**测试开始和测试结束**。每运行一次测试类，就会运行一次 `setup_class()` 和 `teardown_class()`，不关心测试类内有多少个测试函数。

# 使用 fixture 来构建初始化函数和复原函数

标记固定的工厂函数，在其他函数，模块，类或整个工程调用它时会被激活并优先执行，通常会被用于完成预置处理和重复操作。在测试执行时工厂函数 fixture 的执行顺序高于 setup。我们可以通过使用 yield 关键字来实现 setup 和 teardown 操作。

```python
@pytest.fixture()
def open():
    print("打开浏览器，并且打开百度")
    yield
    print("执行teardown")
```

- 如果其中一个用例出现异常，不影响 yield 后面的 teardown 执行，并且全部用例执行完之后，yield 呼唤 teardown 操作。
- 如果在 setup 就异常了，那么是不会去执行 yield 后面的 teardown 内容了。
