---
title: 如何生成 Python 的依赖文件 requirements
date: 2024-12-25T16:08:57+08:00
author: LiangMingJian
---

# 需求

在移植项目时，通过需要 requirements 文件来安装所有的依赖库。

# 使用

```
pip install -r requirements.txt
```

# 通过 pip 生成

```
pip freeze > requirements.txt
```

# 通过 pipreqs 生成

```
pip install pipreqs
pipreqs ./ --encoding=utf8 --force
```
