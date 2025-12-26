---
title: 如何在 PyQt5 中进行焦点控制
date: 2024-12-25T16:06:22+08:00
author: LiangMingJian
---

# 焦点控制

在 Qt 中，焦点是指用户当前正在与之交互的控件，可以通过按 Tab 键或者鼠标单击等方式进行切换。

```python
setFocus() # 设置指定控件获取焦点
setFocusPolicy(Policy)  # 设置焦点获取策略
clearFocus()    # 取消焦点
FocusWidget()   # 获取子控件当前聚焦的控件
FocusNextChild()    # 聚焦下一个子控件
FocusPrevious() # 聚焦上一个子控件
FocusNextPreviousChild(bool) # True:下一个   False:上一个
setTabOrder(pro_widget,next_widget)    # 静态方法 设置子控件获取焦点的先后顺序
```

# EX.组件上面的焦点控制

```python
Qt.TabFocus() # 可以通过 Tab 键获取焦点
Qt.ClickFocus() # 可以通过被单击获取焦点
Qt.StrongFocus()    # 可以通过上面两种方式获取焦点
Qt.NoFocus()    # 不能通过上面两种方式获取焦点
```
