---
title: PyQt5 QFileDialog 文件提示框
date: 2020-12-20
author: LM
---

## 1.QFileDialog 类

QFileDialog 是 PyQt 的一个文件管理器窗口，通过该窗口，用户可用打开文件或目录。

## 2.调用文件对话框获取文件信息

```python
QFileDialog.getExistingDirectory()   # 返回选中的文件夹路径
QFileDialog.getOpenFileName()        # 返回选中的文件路径
QFileDialog.getOpenFileNames()       # 返回选中的多个文件路径
QFileDialog.getSaveFileName()        # 存储文件
```

## 3.返回选中的文件夹路径

```python
QFileDialog.getExistingDirectory(None, "请选择文件夹路径", "D:\\Qt_ui")
QFileDialog.getExistingDirectory(self, "请选择文件夹路径", "D:\\Qt_ui")
# 第一个参数,有self的话用self,没有的话用None
# 第二个参数,设置窗口名
# 第三个参数,设置默认打开路径
```

## 4.获取单个文件的路径

```python
QFileDialog.getOpenFileName(myWin, directory='.', filter='Excel files(*.xlsx ; *.xls)')
# 第一个参数: parent 用于指定父组件,PS: 很多Qt组件的构造函数都会有这么一个parent参数,并提供一个默认值0
# 第二个参数: caption 是对话框的标题
# 第三个参数: dir 是对话框显示时默认打开的目录,"." 代表程序运行目录,"/" 代表当前盘符的根目录
# 第四个参数: filter 是对话框的后缀名过滤器,支持正则
# 第六个参数: options,是对话框的一些参数设定,比如只显示文件夹等 
```

## 5.获取多文件路径实例

```python
QFileDialog.getOpenFileNames(None, "请选择要添加的文件", path, "Text Files (*.xls;*.xlsx);;All Files (*)")
# 第四个参数,列出可以进行筛选的参数,第一个是默认的,多个用双分号分开
```