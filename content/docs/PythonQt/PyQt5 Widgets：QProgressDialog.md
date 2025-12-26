---
title: PyQt5 Widgets：QProgressDialog
date: 2024-12-25T16:07:17+08:00
author: LiangMingJian
---

# 概述

QProgressDialog 是 Qt 提供的进度条组件。

进度条使用 steps 的概念。在指定最小和最大可能的 step 值后，它将显示已经完成的 step 的百分比。

百分比是通过将进度`(value() - minimum()) / (maximum() - minimum())`来计算。用户可以使用 `setMinimum()` 和 `setMaximum()` 指定最小和最大 steps，默认值是 0 和 99。

需要注意的是，**如果最小值和最大值都设置为 0，那么栏会显示一个繁忙的指示符，而不是步骤的百分比**。

# 支持的方法

```python
setMinimum()  # 设置操作中的 steps 数量
setMaximum()  # 设置操作中的 steps 数量
setValue()  # 任意选择步数
setAutoReset()  # 自动重置
setAutoClose()  # 自动关闭
setRange(0,num)  # 设置最小和最大值
wasCanceled()  # 是否按下取消按钮
```

# 示例

```python
def showDialog(self):
    num = int(self.edit.text())
    progress = QProgressDialog(self)
    progress.setWindowTitle("请稍等")  
    progress.setLabelText("正在操作...")
    progress.setCancelButtonText("取消")
    progress.setMinimumDuration(5)
    progress.setWindowModality(Qt.WindowModal)
    progress.setRange(0,num) 
    for i in range(num):
        progress.setValue(i) 
        if progress.wasCanceled():
            QMessageBox.warning(self,"提示","操作失败") 
            break
        else:
            progress.setValue(num)
            QMessageBox.information(self,"提示","操作成功")
```
