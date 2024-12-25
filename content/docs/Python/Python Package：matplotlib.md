---
title: Python Package：matplotlib
date: 2024-12-25T16:11:32+08:00
author: LiangMingJian
---

# 概述

matplotlib 是 Python 提供的绘图工具，可以帮助用户通过可视化的方式查看或分析数据。

```python
pip install matplotlib
```

# 使用

```python
import matplotlib.pyplot as plt  # 约定俗成的写法 plt
import numpy as np

X=np.linspace(-np.pi,np.pi,256,endpoint=True)  # -π，to+π 的 256 个值
# 定义两个函数（正弦&余弦）
C,S=np.cos(X),np.sin(X)
plt.plot(X,C)
plt.plot(X,S)
plt.show()
```

![](/_images/drawingbed/img/202205051045261.png)

# 示例一：在画布上绘制一张图

```python
x=np.linspace(0,10,1000)  # X轴数据
y1=np.sin(x)     # Y轴数据
y2=np.cos(x**2)  # Y轴数据 x^2即x的平方

plt.figure(figsize=(8,4))

plt.plot(x,y1,label="$sin(x)$",color="red",linewidth=2)  # 将$包围的内容渲染为数学公式
plt.plot(x,y2,"b--",label="$cos(x^2)$")
# 指定曲线的颜色和线性，如'b--'表示蓝色虚线（b：蓝色，-：虚线）

plt.xlabel("Time(s)")
plt.ylabel("Volt")
plt.title("PyPlot First Example")

'''
使用关键字参数可以指定所绘制的曲线的各种属性：
label：给曲线指定一个标签名称，此标签将在图标中显示。如果标签字符串的前后都有字符'$'，则Matplotlib会使用其内嵌的LaTex引擎将其显示为数学公式
color：指定曲线的颜色。颜色可以用如下方法表示
       英文单词
       以 # 字符开头的3个16进制数，如 #ff0000 表示红色。
       以0~1的RGB表示，如(1.0,0.0,0.0)也表示红色。
linewidth：指定权限的宽度，可以不是整数，也可以使用缩写形式的参数名lw。
'''

plt.ylim(-1.5,1.5)
plt.legend() # 显示左下角的图例
plt.show()
```

# 示例二：在画布上绘制多张图

如果要绘制多幅图表的话，可以给 Figure 传递一个整数参数指定图表的序号，如果所指定序号的绘图对象已经存在的话，将不创建新的对象，而只是让它成为当前绘图对象。

```python
fig1=plt.figure(2)
plt.subplot(211)
# subplot(211)把绘图区域等分为2行*1列共两个区域，然后在区域1（上区域）中创建一个轴对象
plt.subplot(212)
# 在区域2（下区域）创建一个轴对象
plt.show()
```

![](/_images/drawingbed/img/202205051045627.png)

还可以通过命令再次拆分这些块（相当于 Word 中拆分单元格操作）

```python
f1=plt.figure(5)# 弹出对话框时的标题，如果显示的形式为弹出对话框的话
plt.subplot(221)
plt.subplot(222)
plt.subplot(212)
plt.subplots_adjust(left=0.08,right=0.95,wspace=0.25,hspace=0.45)
# subplots_adjust的操作时类似于网页css格式化中的边距处理，左边距离多少？
# 右边距离多少？这取决于你需要绘制的大小和各个模块之间的间距
plt.show()
```

![](/_images/drawingbed/img/202205051045413.png)

# 示例三：通过 Axes 设置当前图的属性

当画布中绘制图案过多，用户可以使用 Axes 对象选取不同的小模块进行格式化设置。

```python
fig,axes=plt.subplots(nrows=2,ncols=2)
plt.show()
```

![](/_images/drawingbed/img/202205051046698.png)

需要通过命令来操作每个 plot（subplot），设置它们的 title 并删除横纵坐标值。

```python
fig,axes=plt.subplots(nrows=2,ncols=2)
axes[0,0].set(title='Upper Left')
axes[0,1].set(title='Upper Right')
axes[1,0].set(title='Lower Left')
axes[1,1].set(title='Lower Right')

# 通过Axes的flat属性进行遍历
for ax in axes.flat:
    ax.set(xticks=[],yticks=[])
    # xticks和yticks设置为空置
plt.show()
```

![](/_images/drawingbed/img/202205051046875.png)

实际来说，plot 操作的底层就是 Axes 对象的操作，只不过，如果不使用 Axes 而用 plot 操作时，它默认的是 plot.subplot(111)，也就是说 plot 其实是 Axes 的特例。

# 示例四：图像的保存

```python
plt.savefig(r"C:\Users\123\Desktop\save_test.png",dpi=520)
# 默认像素 dpi 是 80
```

# EX.matplotlib 的底层 backend

在 `matplotlib` 中，`frontend` 是我们写的代码，而 `backend` 就是负责显示我们代码所写图形的底层代码。

在不同环境下，相应的 `backend` 后端可能会有所不同，其内容与具体的硬件和显示条件相关。因此在服务器使用`matplotlib`时，可能会因为没有安装图形化和显示相关的包导致出现`backend`错误。为解决`backend`错误的问题，通常需要手动在脚本中指定与服务器相配的`backend`类别。

`backend` 又分为两类，一类是 `interface backend`，又叫做 `interactive backend`，这一类是表示跟显示到屏幕相关的后端；另一类是 `hardcopy backend`，又叫做 `non-interactive backend`，这一类是写入到文件相关的后端。

`non-interactive backend`

![](/_images/drawingbed/img/202205051046100.png)

`interactive backend`

![](/_images/drawingbed/img/202205051047427.png)

```python
import matplotlib
matplotlib.rcsetup.interactive_bk # 获取 interactive backend
matplotlib.rcsetup.non_interactive_bk # 获取 non-interactive backend
matplotlib.rcsetup.all_backends # 获取 所有 backend
```

在代码中，有 4 种方式可以来设置 matplotlib 的 backend，在下面的介绍中，越后面的设置方式，优先级越高，后面的设置会覆盖前面的设置。  

1. 通过设置 `matplotlibrc` 的配置文件来设置。需要注意的是，`matplotlibrc` 文件不一定在你的工程目录下，可以通过`matplotlib.get_configdir()`获取其存放位置，然后添加`backend: WXAgg`这样的内容来进行指定。
2. 通过`MPLBACKEND`环境变量来设置 backend。设置名为`MPLBACKEND='Agg'`的环境变量来进行指定。
3. 通过`-d`选项来设置。在运行脚本时使用`python script.py -d backend`进行指定。
4. 通过`matplotlib.use('Agg')`函数来设置。

{{< details "参考文件" >}} 
1：[ Python 绘图与可视化 @小杜同学的嘚啵嘚 ](https://www.cnblogs.com/dudududu/p/9149762.html)
2：[ Matplotlib 的 backend 浅析 @王云峰 ](https://cloud.tencent.com/developer/article/1559466)
3：[ Matplotlib 接口文档 ](https://matplotlib.org/api/)
{{< /details >}}
