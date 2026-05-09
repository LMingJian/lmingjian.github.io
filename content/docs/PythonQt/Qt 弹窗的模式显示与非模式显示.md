---
title: Qt 弹窗的模式显示与非模式显示
date: 2024-12-25T16:06:37+08:00
author: LiangMingJian
---

# 概述

在 Qt 中，窗口或对话框的显示方式分为‌模式显示‌和‌非模式显示‌两种，它们的区别主要体现在用户交互行为上。

- 模式显示时，用户不能操作其他窗口。
- 非模式显示时，用户可以操作其他窗口。

# 模式显示 Modal

当开发者调用 `exec()` 方法显示一个窗口时，这个窗口会以模式方式显示。

此时，程序会阻塞当前线程，直到这个窗口被关闭。在这个窗口显示期间，用户无法与其它窗口进行交互，不可以切换同程序下的其它窗口。

下面是一个简单的 `exec()` 弹窗示例：

```python
class MyDialog(QDialog):
    """弹窗"""
    dataSend = Signal(str)
    
    def __init__(self):
        super(MyDialog, self).__init__()
        self.ui = Ui_Dialog()
        self.ui.setupUi(self)
        self.ui.Btn01.clicked.connect(self.cancel)
        self.ui.Btn02.clicked.connect(self.confirm)
        
    def cancel(self):
        self.reject()
        
    def confirm(self):
        data = self.ui.lineEdit.text()
        self.dataSend.emit(data)
        self.accept()
        
......
dialog = MyDialog()
dialog.dataSend.connect(self.register)
dialog.exec()
......
```

# 非模式显示 Modeless

当开发者调用 `show()` 方法显示一个窗口时，这个窗口会以非模式方式显示。

此时，程序不会阻塞当前线程，程序会将窗口的控制权即刻返回给调用函数。在这个窗口显示期间，用户可以与其它窗口进行交互，也可以切换同程序下的其它窗口，程序照常运行。

下面是一个简单的 `show()` 弹窗示例：

```python
dialog = MyDialog()
dialog.dataSend.connect(self.register)
dialog.show()
```

# 区别分析

模式显示与非模式显示的区别在于的返回值不同：

- 模式显示的 `exec()` 方法有返回值（返回值是 `QMessageBox.StandardButton` 的枚举值，即按下按钮的值，比如 `QMessageBox.Ok`，`QMessageBox.Cancel` 等）。
- 非模式显示的 `show()` 方法没有返回值。

同时两者在功能上也有所不同：

- 调用 `show()` 的作用仅仅是将 Widget 及其上的内容都显示出来，控制权即刻返回给调用函数。
- 调用 `exec()` 后，调用线程将会被阻塞，直到用户关闭该对话框，期间用户不可以切换同程序下的其它窗口。
