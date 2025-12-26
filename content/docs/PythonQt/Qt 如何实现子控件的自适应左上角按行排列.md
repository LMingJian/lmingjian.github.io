---
title: Qt 如何实现子控件的自适应左上角按行排列
date: 2025-12-23T09:35:48+08:00
author: LiangMingJian
---

# 需求

在 Qt 编程时，有一个长方形 Widget，现在计划在这个 Widget 中添加不同尺寸的子 Widget，这些子 Widget 高度固定，宽度随机。

在添加子 Widget 时，要求子 Widget 从父控件的左上角开始排列，从左到右，从上到下。同时，可以根据子 Widget 的宽度，自动统计每一行的子 Widget 的数量，保证每一行所展示的子 Widget 都不会因为过长而溢出画面。

最终结果如下图所示：

![](_images/drawingbed/img/Pasted%20image%2020251223094458.png)

# 界面实现

**创建父控件**

创建一个主窗口应用，在主窗口应用中添加父控件。

为了在子控件向下溢出时可以滚动查看，因此父控件需要使用 QScrollArea，在 QtDesigner 中，选择下图所示的组件加入：

![](_images/drawingbed/img/Pasted%20image%2020251223112541.png)

加入后，将主窗口布局设置为水平布局，将 QScrollArea 的布局设置为栅栏（网格）布局。

> 在 QtDesigner 中，布局的设置可以通过右键对象来实现。

> 需要注意，在 QtDesigner 中，QScrollArea 在没有子组件的时候无法设置布局，因此可以先加入一个按钮，然后再设置布局，最后删除这个按钮，恢复原状。

![](_images/drawingbed/img/Pasted%20image%2020251223113114.png)

最终，父控件与主窗口的结构关系如下图所示。

![](_images/drawingbed/img/Pasted%20image%2020251223112835.png)

**创建子控件**

创建一个 QFrame 组件，子控件将在这里设置。

![](_images/drawingbed/img/Pasted%20image%2020251223113623.png)

子控件要求进行信息展示，因此直接添加一个 QLabel 控件即可，布局推荐使用水平。

![](_images/drawingbed/img/Pasted%20image%2020251223113905.png)

同时，子控件要求高度固定，宽度随机，因此还需要限定控件的最大最小尺寸。在这里，我们可以直接限定 QFrame 这个父组件的最大最小。

> 注意，这里只需要限定高度。

![](_images/drawingbed/img/Pasted%20image%2020251223114022.png)

> 特别的，为了美观，可以为子控件设置 QCSS。

```css
QLabel {
    background-color: qlineargradient(x1: 0, y1: 0, x2: 1, y2: 1, stop: 0 #ffffff, stop: 1 #f0f9ff);
    border: 1px solid #000000;
}

QLabel:hover {
    background-color: qlineargradient(x1: 0, y1: 0, x2: 1, y2: 1, stop: 0 #f8f9fa, stop: 1 #e6f7ff);
    border: 2px solid #000000;
}
```

# 逻辑实现

在上面的父控件 QScrollArea 中，我们使用了栅栏布局，我们以一个 Button 充当子控件，先看看栅栏布局的效果满不满足我们的需求。

![](_images/drawingbed/img/Pasted%20image%2020251223115401.png)

不难从上面看出，栅栏布局将整个主窗口分为了 3 行 2 列，每个按钮子控件都均匀的放置对应的中心位置。

但这个显然不满足我们的需求，我们要的是子控件从左上角开始，左往右，从上到下的排列，而不是这样的平均分布。

**那怎么实现呢？我们可以使用间隔 Spacers，将空白位置填充，让组件不平均分布**。

![](_images/drawingbed/img/Pasted%20image%2020251223120001.png)

在 QtDesigner 中，我们将一个水平间隔（HorizontalSpacer）和垂直间隔（VerticalSpacer）分别放置在子控件的左边和下边，如下图所示。

![](_images/drawingbed/img/Pasted%20image%2020251223120142.png)

可以看见，现在的子控件就是从左上角开始，左到右，从上到下排列了。

**但这样处理还是不能满足我们的需求，因为当按钮尺寸不一致时，每行的宽度还是固定的，不能根据每行的内容自适应宽度展示**。

![](_images/drawingbed/img/Pasted%20image%2020251223153158.png)

那怎么样才能避免这个问题？

**不难想象，如果我们把每行的按钮都通过一个水平布局（Horizontal Layout）进行包裹，然后在水平布局最后添加一个水平间隔（HorizontalSpacer）填补空白，那不就避免网格布局多行宽度一致的问题了吗**。

此时，我们只用到网格布局的第一列，然后因为每行都单独使用一个水平布局，因此每行插入的按钮数量都是独立的，可以多也可以少，相互间不会影响各自的宽度。

![](_images/drawingbed/img/Pasted%20image%2020251223154229.png)

这个界面的对象结构大致如下：

![](_images/drawingbed/img/Pasted%20image%2020251223154304.png)

> 当然，你应该可以发现，父控件 QScrollArea 在这里不使用网格布局也是可以的，因为只用到第一列，那这个结构其实就是垂直布局，因此，这里的网格布局替换为垂直布局也是没问题的。

**经过上述的研究，我们可以梳理出——子控件自适应从左上角按行排列的实现逻辑**：

- **第一步**：构建一个父控件 QScrollArea（支持溢出后滚动），布局选择网格布局或垂直布局（组件按行排列）。
- **第二步**：在父控件 QScrollArea 的第 1 行（如果是网格布局，则这里的位置是 0,0，第 1 行 1 列）添加一个水平布局组件（Horizontal Layout）。
- **第三步**：统计计算每个待插入子控件的宽度，然后计算每一行至多可以插入多少个子控件不会导致父控件 QScrollArea 水平溢出。
- **第四步**：向水平布局组件（Horizontal Layout）插入上面计算出的子控件数量。
- **第五步**：重复操作三和四，直到所有的子控件都插入完毕。
- **第六步**：在有插入子控件的最后一行下一行插入一个垂直间隔，填补多余的空白位置。

通过代码实现上述六步操作，即可完成目标需求。

# 代码实现

父控件的初始化代码这个不进行给出，通过 QtDesigner 直接生成即可，无需什么额外配置。

**父控件通过以下代码进行调用**：

```python
# 父控件 QScrollArea
self.ui.scroll_area

# 父控件 QScrollArea 的网格布局
self.ui.cards_layout
```

子控件因为宽度随机（一般是随字体数量而变化），因此需要额外进行初始化。

**子控件通过以下代码进行初始化**：

```python
class WordCard(QFrame):  
  
    def __init__(self, data, max_width=500, parent=None):  
        # 构建时支持传入 data（展示的文字内容），max_width（最大宽度）
        super().__init__(parent)  
        self.ui = Ui_WordCard()  
        self.ui.setupUi(self)  
  
        # 将传入数据转换为类属性 
        self.word_data = data  
        self.text = f'{self.word_data["word"]} | {self.word_data["translation"]}'  、
  
        # 计算控件的宽度，然后写入类属性 width
        # 获取控件的字体属性 
        font_metrics = QFontMetrics(self.ui.word_label.font())  
        # 计算这个字体下文字内容的宽度，加上 30 单位的边距宽度
        font_width = font_metrics.horizontalAdvance(self.text) + 30  
        # 如果上述计算的宽度小于输入的最大宽度，则将计算宽度写入类属性 width
        self.width = font_width if font_width <= max_width else max_width  
        
        # 将控件大小设置为计算宽度 
        self.setMaximumWidth(self.width)  
        # 将控件内容设置为文字内容
        self.ui.word_label.setText(self.text)
```

**在主窗口中，构建以下方法函数，实现上文中的三，四，五步，以实现最终需求**。

```python
def refresh_cards(self):
    """刷新卡片显示"""
    # 获取父控件的最大宽度
    scroll_width = self.ui.scroll_area.width()
    # 设置父控件当前剩余的可用宽度
    current_width = scroll_width
    # 设置当前使用的行数
    current_layout = 0
    # 水平布局管理器，管理需要插入的布局（需要使用数组管理，通过行数提取不同行的布局，然后插入子控件）
    horizontal_layouts = [QHBoxLayout()]
    # 在父控件的网格布局中插入第一行的水平布局，位置（0, 0），1 行 1 列
    self.ui.cards_layout.addLayout(horizontal_layouts[current_layout], 0, 0)
    # 从数据字典 self.data 中获取数据
    for key, value in self.data.items():
        # 将数据字典的数据传入子控件构建函数，scroll_width - 50 将最大宽度设置为父控件宽度减去 50 单位的空余
        card = WordCard(value, scroll_width - 50)
        # 将当前的可用宽度减去待添加的子控件宽度，+6 边距
        current_width -= (card.width + 6)
        # 当可用宽度大于 0 时，说明可以插入这一个元素
        if current_width > 0:
            # 可以插入时，在当前使用的水平布局中插入这个元素
            horizontal_layouts[current_layout].addWidget(card)
        else:
            # 不可以插入时，在当前使用的水平布局中插入一个水平间隔（HorizontalSpacer）填补空白
            horizontal_layouts[current_layout].addSpacerItem(QSpacerItem(40, 20, QSizePolicy.Expanding, QSizePolicy.Minimum))  # noqa  
            # 将当前行数加 1
            current_layout += 1
            # 在水平布局管理数组中添加一个新的水平布局
            horizontal_layouts.append(QHBoxLayout()) 
            # 将新的水平布局插入父控件的网格布局中，位置（current_layout, 0），current_layout 行 1 列（下一行，但列数不变） 
            self.ui.cards_layout.addLayout(horizontal_layouts[current_layout], current_layout, 0)
            # 在新一行的水平布局中插入子控件
            horizontal_layouts[current_layout].addWidget(card)  
            # 重置当前可用宽度为父控件最大宽度，然后再减去当前插入元素的宽度
            current_width = scroll_width - (card.width + 6)
    # 在所有元素都插入完成后，在最后一行插入一个水平间隔（HorizontalSpacer）填补空白
    horizontal_layouts[current_layout].addSpacerItem(QSpacerItem(40, 20, QSizePolicy.Expanding, QSizePolicy.Minimum))  # noqa
    # 构建一个垂直间隔（VerticalSpacer）
    vertical_spacer = QSpacerItem(20, 40, QSizePolicy.Minimum, QSizePolicy.Expanding)  # noqa
    # 在插入元素的最后一行的下一行，插入上面创建的垂直间隔，填补剩余的空白
    self.ui.cards_layout.addItem(vertical_spacer, current_row+1, 0)  
```

在主窗口中，调用上述函数方法，既可以在目标父控件中，自动的排列所需要的数据，无需考虑数据的长度问题，也不用担心数据全部都垂直居中，完成目标需求。

# 拓展阅读

**清理父控件**

一般来说，上面的实现代码不可能只执行一次，因此在每次执行前，一般都建议先清理父控件内的所有控件，保证父控件下没有任何内容后才继续执行。

清理的代码一般推荐这样编写：

```python
# 清理代码
def clean_cards(self, widget):
    # 遍历目标控件的所有子控件
    while widget.count():
        # 获取第一个子控件
        child = widget.takeAt(0)
        if child.widget():
            # 如果是控件，则删除控件
            child.widget().deleteLater()
        elif child.layout():
            # 如果是布局，则迭代执行清理代码，删除控件
            self.clean_cards(child)

# 调用
def refresh_cards(self):
    self.clean_cards(self.ui.cards_layout)
    ...
```

**更强大的功能**

在上面代码的基础上，不难发现，还可以通过监听父控件的宽度变化，当宽度变化到一定程度后，可以进行重绘，实现更强大的功能。
