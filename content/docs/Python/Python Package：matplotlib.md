---
title: Python Package：matplotlib
date: 2024-12-25T16:11:32+08:00
author: LiangMingJian
---

# 概述

> 第三方库

Matplotlib 是 Python 中最著名的 2D 绘图库，主要用于在 Python 中创建静态、动画和交互式可视化数据图表。

Matplotlib 支持从简单的折线图、柱状图到复杂的热力图、3D 图等多种图表类型，其被广泛应用于数据分析、科学计算和商业报告等领域。

```python
pip install matplotlib
```

# 基本使用

Matplotlib 的绘图主要使用到图表方法 `plot()` 和展示方法 `show()`，首先通过图表方法 `plot()` 传入目标图表的 X 轴和 Y 轴数据绘制折线图，然后通过 `show()` 方法进行展示。

> 常见的图表支持有：折线图 `plot()`，散点图 `scatter()`，柱状图 `bar()`，饼图 `pie()`，直方图 `hist()`，箱线图 `boxplot()`。

> 所有支持的图表请查看：[ Examples ](https://matplotlib.org/stable/gallery/index.html)

比如下述代码，通过 `np.linspace()` 生成一个在指定区间内等距的数值序列，然后用该序列作为图表的 X 轴数据。接着使用 `np.cos()` 和 `np.sin()` 生成对应 X 轴数据的余弦数据、正弦数据，用来作为图表的 Y 轴数据。

最后将上述 X 轴数据与 Y 轴数据传入 `plot()`，并使用 `show()` 进行展示，得到图表。

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
# 生成范围在 -pi 到 pi 间等距的 256 个数据
# np.linspace(start, stop, num=50, endpoint=True, retstep=False,
#             dtype=None, axis=0, *, device=None)
# np.linspace(起始，结束，样本数量，是否包含结束值，是否返回步长，
#             数据类型，轴，设备类型)
x_axis = np.linspace(-np.pi, np.pi, 256)  

# 生成序列 x_axis 对应的余弦数据序列
y_cos = np.cos(x_axis)  

# 生成序列 x_axis 对应的正弦数据序列
y_sin = np.sin(x_axis) 

# 设置 X 轴和 Y 轴
plt.plot(x_axis, y_cos)  
plt.plot(x_axis, y_sin)  

# 展示图表
plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904113734.png)

# 设置图表的样式

在 `plot()` 方法中，提供多种参数供开发者在绘制图表时设置样式。

常用的参数包括：

- **颜色**：`color` 或 `c`，支持颜色对应的英文，如 `red, blue`，也支持 hex RGB 格式字符，如 `#0f0f0f`。详细支持可查看：[ Specifying colors ](https://matplotlib.org/stable/users/explain/colors/colors.html)。
- **线的样式**：`linestyle` 或 `ls`，支持 `-, --, -., :`。
- **线的宽度**：`linewidth` 或 `lw`，浮点数。
- **线的图标**：`label`，字符串，如果以 `$` 包裹，则以 LaTex 格式解析为数学公式。该标签需要结合 `plt.legend()` 方法展示。
- **数据标记**：`marker`，支持 `., o, v, ^, *` 等，详细支持查看：[ matplotlib.markers ](https://matplotlib.org/stable/api/markers_api.html)

**特别的，对于数据标记，线的样式，颜色，支持通过格式字符串 fmt 进行传递。**

格式字符串如下所示，传入对应参数，`plot()` 会自动解析，并生成所需图表样式。

比如：`.-g`（用点标记数据，实线，绿色），`--r`（虚线，红色），`b`（蓝色），`--`（虚线），`o`（用圈标记数据）。

```python
fmt = '[marker][line][color]'
```

注意，关键字参数优先级高于格式字符串，同时传入时，以关键字为准。

> 所有参数可查看：[ matplotlib.pyplot.plot ](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html)

除了上述样式设置，还可以通过 `plt.xlabel(), plt.ylabel(), plt.title()` 来设置图表的 X 轴标题，Y 轴标题，以及图表标题。

如下面代码所示：

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi,np.pi, 256, endpoint=True)  
y_cos = np.cos(x_axis)  
y_sin = np.sin(x_axis)  

# 以关键字参数设置样式
plt.plot(x_axis, y_cos, label="$cos(x)$", 
		 color="red", linestyle="-.", linewidth=1)
# 以格式化字符设置样式
plt.plot(x_axis, y_sin, 'x--g', label="sin(x)")
# 显示线的图标
plt.legend()
# 设置 X 轴标题
plt.xlabel("X-axis")
# 设置 Y 轴标题
plt.ylabel("Y-axis")  
# 设置图表标题
plt.title("Example")  

plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904113832.png)

# 设置画布大小与背景边框

`plt.figure()` 是 Matplotlib 中用于创建或激活图形窗口方法，其支持创建多个图形窗口，支持设置画布大小，支持设置分辨率，以及画布背景以及边框。

常用的参数包括：

- `num`：图形标识符（整型 `int` 或字符串 `str`），用于指定后续 `plt` 使用的图形窗口，没有对应标识符的图表则会新建。
- `figsize`：画布尺寸 `(w, h)`，单位英寸，默认 `(6.4, 4.8)`。
- `dpi`：分辨率，默认 100。
- `facecolor`：画布背景色，默认白色。
- `edgecolor`：边框颜色，默认无边框。
- `linewidth`：边框宽度，浮点数。

比如下述代码，生成两个 `(5, 5)` 的图表窗口，一个背景颜色为白色，一个灰色，一个边框红色，一个边框蓝色：

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi,np.pi, 256, endpoint=True)  
y_cos = np.cos(x_axis)  
y_sin = np.sin(x_axis)  
  
# 第一个窗口  
plt.figure(1, figsize=(5, 5), dpi=100,  
           facecolor='white', edgecolor='red', linewidth=2)  
plt.plot(x_axis, y_cos)  
  
# 第二个窗口  
plt.figure(2, figsize=(5, 5), dpi=100,  
           facecolor='lightgray', edgecolor='blue', linewidth=2)  
plt.plot(x_axis, y_sin)  
  
plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904113933.png)

![](_images/drawingbed/img/Pasted%20image%2020250904113924.png)

# 在同一画布绘制多张图表

在 Matplotlib 中，如果要在同一画布上绘制多张图表，需要使用 `plt.subplot()` 方法，将画布划分为多个图表区域，然后在指定的图表区域内使用作图。

子图方法 `subplot()` 按顺序可传入三个参数用以划分画布的行数，列数，以及指定后续使用的子图位置（从左到右，从上到下编号）。

比如下述代码，将画布分为上下两个区域，分别绘制余弦和正弦函数：

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi,np.pi, 256, endpoint=True)  
y_cos = np.cos(x_axis)  
y_sin = np.sin(x_axis)  

# 设置 2 行 1 列的第 1 个子图 
plt.subplot(2, 1, 1)   
plt.plot(x_axis, y_cos) 

# 设置 2 行 1 列的第 2 个子图 
plt.subplot(2, 1, 2)    
plt.plot(x_axis, y_sin)  

plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904113543.png)

特别注意，划分子图时，**子图的序号是按最大划分，从左到右，从上到下计算的**。

比如下述代码，先划分 2 行 1 列，再划分 2 行 2 列，此时绘制 3 个图形，那么对应图形的序号应该是 1，3，4，没有 2，这是因为 1，2 是同一行：

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi, np.pi, 256, endpoint=True)  
y_cos = np.cos(x_axis)  
y_sin = np.sin(x_axis)  
  
# 设置 2 行 1 列的第 1 个子图  
plt.subplot(2, 1, 1)  
plt.plot(x_axis, y_cos)  
  
# 设置 2 行 2 列的第 3 个子图  
plt.subplot(2, 2, 3)  
plt.plot(x_axis, y_sin)  

# 设置 2 行 2 列的第 4 个子图  
plt.subplot(2, 2, 4)  
plt.plot(x_axis, y_cos)  

plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904115111.png)

# 通过 Axes 对象操作多个子图

上面绘制子图时，对每个子图的操作都需要先使用 `plt.subplot()` 指定，有点过于麻烦。在图表更多的时候，有没有简单点的方法呢？

答案显示是有的，用户可以使用 `plt.subplot()` 的兄弟方法 `plt.subplots()`，返回 Axes 对象来选取不同的图表进行设置。

`plt.subplots()` 会返回一个 `(Figure, Axes)` 元组数据。Figure 对象是窗口容器对象，可以通过其完成对窗口或画布的设置。Axes 对象是绘图区域对象，可以通过其配置子图的样式，结构以及数据。

比如下述代码，分别对 2 行 2 列的 4 个子图进行配置：

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi, np.pi, 256, endpoint=True)  
y_cos = np.cos(x_axis)  
y_sin = np.sin(x_axis)  
  
# 获取 Figure, Axes 对象
figure, axes = plt.subplots(2, 2)  

# 通过 Figure 调整画布大小
figure.set_size_inches(6.4, 6.4)  

# 分别设置不同区域的子图
axes[0, 0].set(title='Upper Left')  
axes[0, 0].plot(x_axis, y_cos)  
  
axes[0, 1].set(title='Upper Right')  
axes[0, 1].plot(x_axis, y_cos)  
  
axes[1, 0].set(title='Lower Left')  
axes[1, 0].plot(x_axis, y_sin)  
  
axes[1, 1].set(title='Lower Right')  
axes[1, 1].plot(x_axis, y_sin)  
  
plt.show()
```

![](_images/drawingbed/img/Pasted%20image%2020250904134230.png)

# 保存图像

```python
import matplotlib.pyplot as plt  
import numpy as np  
  
x_axis = np.linspace(-np.pi, np.pi, 256)  
y_sin = np.sin(x_axis)  
  
plt.plot(x_axis, y_sin)  

# 保存图像
plt.savefig(r"save.png", dpi=100)
```

# 拓展阅读：后端 Backend

在 Matplotlib 中，frontend 是我们写的代码，而 backend 就是负责渲染并显示我们代码所绘制内容的底层代码。

在不同环境下，后端 backend 也可能会有所不同，其内容与具体的硬件和显示条件相关。

**因此在服务器（Linux 系统）使用 Matplotlib 时，可能会因为没有安装图形化工具或显示工具导致 backend 错误，从而无法绘图。**

此时，为解决 backend 错误的问题，通常需要手动在脚本中指定与服务器（Linux 系统）相配的 backend 类别。

backend 一般分为以下两类：

- **Static backends**：这一类是写入到文件的后端。
- **Interactive backends**：这一类是显示到屏幕的后端。

> 具体内容可查阅：[ Backends ](https://matplotlib.org/stable/users/explain/figure/backends.html)

![](_images/drawingbed/img/Pasted%20image%2020250904135900.png)

![](_images/drawingbed/img/Pasted%20image%2020250904135925.png)

在代码中，有 3 种方式可以用来设置 Matplotlib 的 backend，越后面的设置方式，优先级越高，同时后面的设置会覆盖前面的设置。  

1. **通过配置文件设置**：通过往配置文件 `matplotlibrc` 里添加 `backend: qtagg` 来指定后端。可以通过 `matplotlib.matplotlib_fname()` 获取配置文件所在。
2. **通过配置环境变量设置**：在环境变量中添加 `MPLBACKEND=qtagg` 指定后端。
3. **通过内置代码设置**：在代码开头添加 `matplotlib.use('qtagg')` 指定后端。

> 不推荐在运行脚本时使用 `python script.py -d backend` 进行指定，虽然也有效。

————————————

> [ Matplotlib 官网 ](https://matplotlib.org/)
