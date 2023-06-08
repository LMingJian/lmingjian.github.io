---
title: "Allure 的使用"
date: 2021-01-05
author: LMingJian
---

## 1.简介

Allure 是一个轻量级、灵活的、支持多语言、支持多平台的测试报告工具，其可以提供详尽的测试报告、测试步骤、log、数据统计报告。

Allure 是一个应用工具，并不是程序中的一个模块，需要单独进行下载安装。

在编写 Pytest 代码时，需要使用`pip install allure-pytest`安装对应的模块，来生成支持 Allure 调用的数据报告。这些数据报告会交由 Allure 来进行统计展示。

文档：[ Allure Framework ](https://docs.qameta.io/allure-report/)，下载：[ allure2@github ](https://github.com/allure-framework/allure2/releases) 

## 2.命令行使用

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

## 3.Pytest 使用：生成报告

```shell
pytest --alluredir=/tmp/my_allure_results  # 执行测试并生成报告
```

## 4.Pytest 使用：附件

```python
# 使用 allure.attach() 来插入一段自己写的 HTML 
# 或 allure.attach.file() 来导入一个已存在的 HTML 文件
allure.attach(body, name, attachment_type, extension)
allure.attach.file(source, name, attachment_type, extension)
"""
body：要显示的内容（附件）
name：附件名字
attachment_type：附件类型，是 allure.attachment_type 里面的其中一种
extension：附件的扩展名（比较少用）
source：文件路径，相当于传一个文件
"""
```

## 5.Pytest 使用：语法糖

```python
@allure.feature('功能名称')
@allure.issue("测试网站") 
@allure.link("链接")
@allure.testcase("测试ID")
@allure.description("说明")
@allure.story('子功能名称') 
@allure.title('标题')
@allure.step('步骤')
@allure.severity('级别')
```

## 6.Pytest 使用：用例的描述

```python
# 注释即为描述
"""
描述内容
"""
```
