---
title: PyQt5 程序的启动
date: 2020-12-26
author: LM
---

## 1.启动的代码

PyQt5 编写的应用程序通常使用以下代码进行启动。

```python
apps = QApplication(sys.argv)
myWin = MyWindow() # 创建Qt类
myWin.show() # 展示
sys.exit(apps.exec_())
```

## 2.解析

- `QApplication(sys.argv)`：运行程序时候获取命令行参数，使Qt的各种类能获取到程序返回的参数
- `sys.exit(apps.exec_())`：使得指程序一直循环运行直到主窗口被关闭终止进程（如果没有这句话，程序运行时会一闪而过），作用是给予系统一个结束程序的状态判断

