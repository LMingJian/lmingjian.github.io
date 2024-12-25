---
title: Python 中的 import 是什么
date: 2024-12-25T16:11:17+08:00
author: LiangMingJian
---

# 什么可以被 import ?

因为 Python 中一切都是对象，都属于 object，所以任何东西都可以被 import，在这些不同的对象中，我们经常使用到的结构是模块 Module 和包 Package。它们在 Python 的底层都是以 PyModuleObject 的结构体实例存在的，类型为 PyModule_Type，而在 Python 中则表现为一个对象 `<class 'module'>` 。

# 绝对导入

绝对路径要求我们必须从最顶层的文件夹开始，为每个包或每个模块提供出完整详细的导入路径：

```python
from package1 import mudule1
from package1.module2 import Fx
from package2 import Cx
from package2.subpackage1.module5 import Fy
```

# 相对导入

相对导入则要求我们给出相对与当前位置，想导入资源所在的位置。另外，相对导入又分为隐式相对导入和显式相对导入两种，比如我们想在`package2/module3.py`中引用`module4`模块，我们可以这么写：

```python
# package2/module3.py

import module4 # 隐式相对导入
from . import module4 # 显式相对导入
from package2 import module4 # 绝对导入
```

代码中`.`表示当前文件所在的目录，如果是`..`就表示该目录的上一层目录。可以看出，隐式相对导入相比于显式相对导入无非就是隐含了当前目录这个条件，不过由于这样会容易引起混乱，所以在 [PEP 328](https://link.zhihu.com/?target=https%3A//www.python.org/dev/peps/pep-0328/) 的时候被正式淘汰。

注意在跨多个文件导入包时，需注意相对导入寻找相对位置的算法是基于`__name__`和`__package__`变量的值。大部分时候，这些变量不包含任何包信息。比如：当`__name__=__main__`，`__package__=None`时，Python 解释器不知道模块所属的包。在这种情况下，相对引用会认为这个模块就是顶级模块，而不管模块在文件系统上的实际位置。

# import 的流程

调用关键字`import`的时候主要会触发两个操作：**搜索**和**加载**（也可以理解为搜索模块在哪里和把找到的模块加载到某个地方），Python 会在两个地方寻找这个模块，第一是`sys.path`（通过运行代码`print(sys.path)`查看），第二个地方就是运行文件所在的目录。

# 通过 import 导入

```python
import a
print(type(a))
```

打印`<class 'module'>`，上述代码加载的是一个模块对象，使用的是另一个模块的内存，其运行逻辑如图：

![](/_images/drawingbed/img/202205051041367.jpg)

# 通过 from ... import ... 导入

```python
from a import zx
print(type(zx))
```

打印`<class 'method'>`，加载的是方法，直接将内容复制一份，使用自己的内存，其运行逻辑如图：

![](/_images/drawingbed/img/202205051041731.jpg)

{{< details "参考文件" >}} 
1：[ Python中import的用法  @门书生 ](https://zhuanlan.zhihu.com/p/63143493)
2：[ 聊聊Python Import System？@没有50CM手臂 ](https://zhuanlan.zhihu.com/p/348559778)
{{< /details >}}
