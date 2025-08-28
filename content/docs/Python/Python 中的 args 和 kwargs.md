---
title: Python 中的 *args 和 **kwargs
date: 2025-07-07T11:39:21+08:00
author: LiangMingJian
---

# Code

在Python中的代码中经常会见到这两个词 args 和 kwargs，前面通常还会加上一个或者两个星号。其实这只是编程人员约定的变量名字，args 是 arguments 的缩写，表示位置参数；kwargs 是 keyword arguments 的缩写，表示关键字参数。这其实就是 Python 中可变参数的两种形式，并且 [*args](https://zhida.zhihu.com/search?content_id=10350999&content_type=Article&match_order=1&q=%2Aargs&zhida_source=entity) 必须放在 [**kwargs](https://zhida.zhihu.com/search?content_id=10350999&content_type=Article&match_order=1&q=%2A%2Akwargs&zhida_source=entity) 的前面，因为位置参数在关键字参数的前面。

## *args的用法

*args就是就是传递一个可变参数列表给函数实参，这个参数列表的数目未知，甚至长度可以为0。下面这段代码演示了如何使用args

```python
def test_args(first, *args):
    print('Required argument: ', first)
    print(type(args))
    for v in args:
        print ('Optional argument: ', v)

test_args(1, 2, 3, 4)
```

第一个参数是必须要传入的参数，所以使用了第一个形参，而后面三个参数则作为可变参数列表传入了实参，并且是作为[元组](https://zhida.zhihu.com/search?content_id=10350999&content_type=Article&match_order=1&q=%E5%85%83%E7%BB%84&zhida_source=entity)tuple来使用的。代码的运行结果如下

```cpp
Required argument:  1
<class 'tuple'>
Optional argument:  2
Optional argument:  3
Optional argument:  4
```

## **kwargs

而**kwargs则是将一个可变的关键字参数的[字典](https://zhida.zhihu.com/search?content_id=10350999&content_type=Article&match_order=1&q=%E5%AD%97%E5%85%B8&zhida_source=entity)传给函数实参，同样参数列表长度可以为0或为其他值。下面这段代码演示了如何使用kwargs

```python
def test_kwargs(first, *args, **kwargs):
   print('Required argument: ', first)
   print(type(kwargs))
   for v in args:
      print ('Optional argument (args): ', v)
   for k, v in kwargs.items():
      print ('Optional argument %s (kwargs): %s' % (k, v))

test_kwargs(1, 2, 3, 4, k1=5, k2=6)
```

正如前面所说的，args类型是一个tuple，而kwargs则是一个字典dict，并且args只能位于kwargs的前面。代码的运行结果如下

```text
Required argument:  1
<class 'dict'>
Optional argument (args):  2
Optional argument (args):  3
Optional argument (args):  4
Optional argument k2 (kwargs): 6
Optional argument k1 (kwargs): 5
```

## 调用函数

args和kwargs不仅可以在函数定义中使用，还可以在函数调用中使用。在调用时使用就相当于pack（打包）和unpack（[解包](https://zhida.zhihu.com/search?content_id=10350999&content_type=Article&match_order=1&q=%E8%A7%A3%E5%8C%85&zhida_source=entity)），类似于元组的打包和解包。

首先来看一下使用args来解包调用函数的代码，

```python
def test_args_kwargs(arg1, arg2, arg3):
    print("arg1:", arg1)
    print("arg2:", arg2)
    print("arg3:", arg3)

args = ("two", 3, 5)
test_args_kwargs(*args)

#result:
arg1: two
arg2: 3
arg3: 5
```

将元组解包后传给对应的实参，kwargs的用法与其类似。

```bash
kwargs = {"arg3": 3, "arg2": "two", "arg1": 5}
test_args_kwargs(**kwargs)

#result
arg1: 5
arg2: two
arg3: 3
```

args和kwargs组合起来可以传入任意的参数，这在参数未知的情况下是很有效的，同时加强了函数的可拓展性。

{{< details "参考文件" >}} 
1：[ Python 中的 \*args 和 \*\*kwargs_@ 知乎 ](https://zhuanlan.zhihu.com/p/50804195)
{{< /details >}}
