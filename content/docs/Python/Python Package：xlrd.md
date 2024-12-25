---
title: Python Package：xlrd
date: 2024-12-25T16:12:55+08:00
author: LiangMingJian
---

# 概述

python 中操作 Excel 表格主要用到 xlrd 和 xlwt 两个库，xlrd 库提供读取功能，xlwt 库提供写入功能，但 python 中不存在对 Excel 表格进行修改的库。

PS：注意在版本 1.3.0 中，这两个库不再支持 xlsx 格式的表格文件，如要使用请指定版本 1.2.0。

```bash
pip install xlrd==1.2.0
```

# 打开文件

```python
import xlrd
data = xlrd.open_workbook(filename)  # 文件名以及路径
```

打开文档后，必须先定位到 sheet，才能进行下一步操作，程序只有在获取对应的 sheet 之后，才能进行对行和列以及单元格的操作。

# 获取工作表

```python
table = data.sheets()[0]          # 通过索引顺序获取
table = data.sheet_by_index(sheet_indx)  # 通过索引顺序获取
table = data.sheet_by_name(sheet_name) # 通过名称获取
names = data.sheet_names()    # 返回 book 中所有工作表的名字
data.sheet_loaded(sheet_name or indx)   # 检查某个sheet是否导入完毕
```

# 对行的操作

```python
nrows = table.nrows  # 获取该 sheet 中的有效行数
table.row(rowx)  # 返回由该行中所有的单元格对象组成的列表
table.row_slice(rowx)  # 返回由该列中所有的单元格对象组成的列表
table.row_types(rowx, start_colx=0, end_colx=None)    # 返回由该行中所有单元格的数据类型组成的列表
table.row_values(rowx, start_colx=0, end_colx=None)   # 返回由该行中所有单元格的数据组成的列表
table.row_len(rowx) # 返回该列的有效单元格长度
```

# 对列的操作

```python
ncols = table.ncols   # 获取列表的有效列数
table.col(colx, start_rowx=0, end_rowx=None)  # 返回由该列中所有的单元格对象组成的列表
table.col_slice(colx, start_rowx=0, end_rowx=None)  # 返回由该列中所有的单元格对象组成的列表
table.col_types(colx, start_rowx=0, end_rowx=None)    # 返回由该列中所有单元格的数据类型组成的列表
table.col_values(colx, start_rowx=0, end_rowx=None)   # 返回由该列中所有单元格的数据组成的列表
```

# 对单元格的操作

```python
table.cell(rowx,colx)   # 返回单元格对象
table.cell_type(rowx,colx)    # 返回单元格中的数据类型
table.cell_value(rowx,colx)   # 返回单元格中的数据
```
