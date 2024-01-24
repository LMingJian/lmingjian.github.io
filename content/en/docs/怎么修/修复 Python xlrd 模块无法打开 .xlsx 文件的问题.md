---
title: 修复 Python xlrd 模块无法打开 .xlsx 文件的问题
date: 2021-03-30
author: LMingJian
---

## BUG 描述

xlrd 模块更新后，无法打开 .xlsx 文件，报错 `xlrd.biffh.XLRDError: Excel xlsx file；not supported`。

## Resolution

xlrd 在更新到了 2.0.1 版本后只支持 .xls 文件，若要打开 .xlsx 文件需要安装旧版 xlrd，执行。

```python
pip uninstall xlrd
pip install xlrd==1.2.0
```

