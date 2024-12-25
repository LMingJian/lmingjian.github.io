---
title: Allure Report 的命令行使用
date: 2024-12-25T11:00:05+08:00
author: LiangMingJian
---

# 简介

Allure 是一个轻量级、灵活的、支持多语言、支持多平台的测试报告工具，其可以提供详尽的测试报告、测试步骤、log、数据统计报告。

Allure 是一个应用工具，并不是程序中的一个模块，需要单独进行下载安装。

在编写 Pytest 代码时，需要使用`pip install allure-pytest`安装对应的模块，来生成支持 Allure 调用的数据报告。这些数据报告会交由 Allure 来进行统计展示。

文档：[ Allure Framework ](https://docs.qameta.io/allure-report/)，下载：[ allure2@github ](https://github.com/allure-framework/allure2/releases) 

# 命令行使用

```python
#----- generate ------------------------------
allure generate DIR -o DIR -c DIR
allure generate /tmp/my_allure_results -o /tmp/result -c
# 参数：-c, --clean 在生成新的 allure 报告之前，先清除该目录
# 参数：-o, --report-dir, --output 指定目录生成 allure 报告

#----- open ------------------------------
allure open DIR
# 参数：-h, --host  指定域名地址
# 参数：-p, --port  指定端口号

#----- serve ------------------------------
allure serve /report/html
allure serve /tmp/my_allure_results -p 33333
# 参数：-h, --host  指定域名地址
# 参数：-p, --port  指定端口号

# 测试执行，在指定目录 DIR 生成 allure 报告与如果目录存在则清除目录
# 测试执行后，pytest 将生成一个支持 allure 使用的数据报表
pytest --alluredir=DIR --clean-alluredir
```
