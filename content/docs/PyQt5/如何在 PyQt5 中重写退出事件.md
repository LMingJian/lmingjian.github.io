---
title: 如何在 PyQt5 中重写退出事件
date: 2024-12-25T16:06:32+08:00
author: LiangMingJian
---

# 代码实现

当用户关闭一个窗口时，在 PyQt 中就会触发一个 QCloseEvent 的事件，正常情况下会直接关闭这个窗口。但在某些时候，我们并不希望这样的事情发生，因此可以重写 QCloseEvent 来变更退出事件。

```python
import sys
from PyQt5 import QtWidgets
from PyQt5.QtGui import QFont
# QtWidgets 不包含 QFont 必须调用 QtGui

class MessageBox(QtWidgets.QWidget):
    def __init__(self,parent = None):
        # parent = None 代表此 QWidget 属于最上层的窗口,也就是 MainWindows.
        QtWidgets.QWidget.__init__(self)
        # 因为继承关系，要对父类初始化
        # 通过 super 初始化父类，__init__() 函数无 self，若直接 QtWidgets.QWidget.__init__(self)，括号里是有self的
        self.setGeometry(300, 300, 1000,1000)  
        # setGeometry() 设置窗口在屏幕上的位置和设置窗口本身的大小。它的前两个参数是窗口在屏幕上的 x 和 y 坐标。后两个参数是窗口本身的宽和高
        self.setToolTip(u'<b>程序</b>提示') 
        # 调用 setToolTip() 方法,该方法接受富文本格式的参数,css 之类。
        QtWidgets.QToolTip.setFont(QFont('华文楷体', 10)) 
        # 设置字体以及字体大小

    # 重写 closeEvent
    def closeEvent(self,event):
        # 函数名固定不可变
        reply=QtWidgets.QMessageBox.question(self,u'警告',u'确认退出?',QtWidgets.QMessageBox.Yes,QtWidgets.QMessageBox.No)
        # QtWidgets.QMessageBox.question(self,u'弹窗名',u'弹窗内容',选项1,选项2)
        if reply==QtWidgets.QMessageBox.Yes:
            event.accept()
            # 关闭窗口
        else:
            event.ignore()
            # 忽视点击 X 事件

app=QtWidgets.QApplication(sys.argv)
window=MessageBox()
window.show()
sys.exit(app.exec_())
```
