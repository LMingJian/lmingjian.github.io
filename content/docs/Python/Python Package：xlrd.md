---
title: Python Package：xlrd
date: 2024-12-25T16:12:55+08:00
author: LiangMingJian
---

# 概述

> 第三方库

在 Python 中，读取 Excel 表格可以使用 pandas 模块、openpyxl 模块或 xlrd 模块。在读取复杂数据，建议使用 pandas 模块或 openpyxl 模块。但如果只是读取一些简单数据，那么使用更轻便简单的 xlrd 模块似乎更好。

xlrd 模块支持对 xls 格式的文件进行读取分析，对于 xlsx 格式的文件，则需要将版本限定在 1.2.0（在版本 2.0+ 后，xlrd 模块移除了对 xlsx 格式文件的支持）。

```python
pip install xlrd==1.2.0
```

# 打开文件并获取工作表

xlrd 模块通过方法 `open_workbook` 打开文件并返回一个 `Book` 对象，通过 `Book` 对象定位到工作表对象后，程序才能对行、列、以及单元格进行操作。

```python
import xlrd

book = xlrd.open_workbook('my.xls')

# 返回文件中所有工作表的名字
names = book.sheet_names()  # ['Sheet1', 'Sheet2', 'Sheet3'] 

# 获取工作表
table = book.sheets()[0]                 # 通过索引顺序获取
table = book.sheet_by_index(0)  # 通过索引顺序获取
table = book.sheet_by_name('Sheet1')   # 通过名称获取
```

# 对单元格的操作

`table.cell(rowx, colx)`，通过工作表单元格方法 `cell()` 定位行列获取单元格对象，可以通过单元格对象获取对应数据的类型和内容。

返回的数据类型以整型表示，其对应关系如下：

- 0：XL_CELL_EMPTY，空单元格
- 1：XL_CELL_TEXT，字符串
- 2：XL_CELL_NUMBER，数值
- 3：XL_CELL_DATE，时间，需要使用 `xlrd.xldate_as_tuple()` 进行转换
- 4：XL_CELL_BOOLEAN，布尔值
- 5：XL_CELL_ERROR，错误值 N/A

```python
import xlrd  
  
book = xlrd.open_workbook('my.xls')  
table = book.sheet_by_name('Sheet1')  
  
cell = table.cell(0, 0)  # 获取 (0, 0) 的数据
print(cell.ctype)  # 打印类型
print(cell.value)  # 打印数值

print(table.cell_type(0, 0))  # 直接返回单元格的类型  
print(table.cell_value(0, 0))  # 直接返回单元格的数据

# 将时间数据转换为可视类型
data = table.cell_value(0, 1)
time = xlrd.xldate_as_tuple(data, datemode=0)  
# datemode 0/1，Windows 使用 0，Mac 使用 1
print(time)  
# (2025, 9, 1, 0, 0, 0)
```

# 对行的操作

xlrd 模块自动识别工作表中所有具有数据的行，有效行数等于最后一个有数据的行的位置，即使这行前面存在多个空白行。行数的记录从 0 开始。

```python
import xlrd  
  
book = xlrd.open_workbook('my.xls')  
table = book.sheet_by_name('Sheet1')  

# 获取工作表的有效行数
nrows = table.nrows  
# 10

# 返回该行有效列数
ncols = table.row_len(0)
# 3

# 返回该行所有的单元格对象组成的列表，start_colx 开始列，end_colx 结束列
cell = table.row(0, start_rowx=0, end_rowx=None) 
# [number:1.0, xldate:45901.0, number:3030.0]

# 返回该行所有数据列表，start_colx 开始列，end_colx 结束列
data = table.row_values(0, start_colx=0, end_colx=None)  
# [1.0, 2025.0, 3030.0]

# 返回该行所有类型列表，start_colx 开始列，end_colx 结束列
data = table.row_types(0, start_colx=0, end_colx=None) 
# array('B', [2, 3, 2])，转换为列表使用：list(data)
```

# 对列的操作

```python
import xlrd  
  
book = xlrd.open_workbook('my.xls')  
table = book.sheet_by_name('Sheet1')  

# 获取列表的有效列数
ncols = table.ncols
# 3

# 返回该列所有的单元格对象组成的列表，start_rowx 开始行，end_rowx 结束行
cell = table.col(0, start_rowx=0, end_rowx=None)
# [number:1.0, ..., number:10.0]

# 返回该列所有数据列表，start_rowx 开始行，end_rowx 结束行
data = table.col_values(0, start_rowx=0, end_rowx=None)
# [1.0, 2.0, ..., 9.0, 10.0]

# 返回该列所有类型列表，start_rowx 开始行，end_rowx 结束行
data = table.col_types(0, start_rowx=0, end_rowx=None)
# [2, 2, 2, 2, 2, 2, 2, 2, 2, 2]
```

————————————

> [ xlrd - 官方文档 ](https://xlrd.readthedocs.io/en/latest/index.html)
