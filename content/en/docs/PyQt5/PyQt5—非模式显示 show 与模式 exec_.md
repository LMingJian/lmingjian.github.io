---
title: PyQt5 非模式显示与模式显示
date: 2020-12-25
author: LM
---

## 1.非模式显示 show()

对话框弹出后，控制权即刻返回给调用函数，在显示期间，用户可以切换同程序下的其它窗口，程序照常运行。

## 2.模式显示 exec_()

对话框弹出后，锁住程序直到用户关闭该对话框为止，函数返回一个 DialogCode 结果。在显示期间，用户不可以切换同程序下的其它窗口。

## 3.模式与非模式

- 模式对话框，就是在弹出窗口的时候，整个程序就被锁定了，处于等待状态，直到对话框被关闭。这时往往是需要对话框的返回值进行下面的操作。如：确认窗口。
- 非模式对话框，在调用弹出窗口之后，调用即刻返回，继续下面的操作。这里只是一个调用指令的发出，不等待也不做任何处理。如：查找框。

两者的返回值不同。exec() 有返回值，show() 没有返回值。其次这两个方法的作用也不同。调用 show() 的作用仅仅是将 widget 及其上的内容都显示出来，控制权即刻返回给调用函数。而调用 exec() 后，调用线程将会被阻塞，锁住程序直到用户关闭该对话框，期间用户不可以切换同程序下的其它窗口直到 Dialog 关闭。
