---
title: 修复 Python xlrd 模块无法打开 .xlsx 文件的问题
date: 2024-12-25T16:10:58+08:00
author: LiangMingJian
---

# BUG 描述

xlrd 模块更新后，无法打开 .xlsx 文件，报错 `xlrd.biffh.XLRDError: Excel xlsx file；not supported`。

# Resolution

xlrd 模块在更新到了 2.0.1 版本后只支持 .xls 文件，若要打开 .xlsx 文件需要安装旧版 xlrd 或换用 pandas 模块。

```python
pip uninstall xlrd
pip install xlrd==1.2.0

pip install pandas
```
