---
title: 如何在 PyQt5 中隐藏窗口的最大最小按钮
date: 2024-12-25T16:06:27+08:00
author: LiangMingJian
---

# 代码实现

在绘制 Qt 界面时，有时候不希望窗口右上角的最大化，最小化和关闭按钮出现，此时可以通过以下这些方法进行移除。

```python
# 1、直接隐藏界面整个头部内容
setWindowFlags(Qt.FramelessWindowHint)

# 2、显示最小化按钮
setWindowFlags(Qt.WindowMinimizeButtonHint)

# 3、显示最大化按钮
setWindowFlags(Qt.WindowMaximizeButtonHint)

# 4、显示最小化和最大化按钮
setWindowFlags(Qt.WindowMinMaxButtonsHint)

# 5、显示关闭按钮
setWindowFlags(Qt.WindowCloseButtonHint)

# 6、固定界面大小尺寸，不能进行缩放（三种方法都可以）
setWindowFlags(Qt.MSWindowsFixedSizeDialogHint)
setFixedSize(width, height)
setMinimumSize(800, 700)   setMaximumSize(800, 700)

# 7、取消最小化和最大化，及关闭按钮（利用固定大小方法）
setWindowFlags(Qt.WindowMaximizeButtonHint | Qt.MSWindowsFixedSizeDialogHint)
```
