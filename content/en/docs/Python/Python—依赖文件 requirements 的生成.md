---
title: Python 依赖文件 requirements 的生成
date: 2023-03-16
author: LM
---

## 1.需求

在移植项目时，通过需要 requirements 文件来安装所有的依赖库。

## 2.使用

```
pip install -r requirements.txt
```

## 3.实现 ：通过 pip 生成

```
pip freeze > requirements.txt
```

## 4.实现 ：通过 pipreqs 生成

```
pip install pipreqs
pipreqs ./ --encoding=utf8 --force
```

