---
title: 修复 Selenium 在使用 executable_path 时出现 DeprecationWarning 的问题
date: 2024-12-25T16:13:42+08:00
author: LiangMingJian
---

# BUG 描述

Selenium 在使用 executable_path 时，程序能正常运行，但会出现 DeprecationWarning 警告的类型错误。

# Resolution

这是由于版本更新，该函数在当前版本被重构，虽然还可以传入参数，但是在之后的某个版本会被删除。

> Executable path has been deprecated please pass in a Service object in Selenium Python。

在更新记录中查询当前版本重构后的函数，发现之前的 executable_path 被重构到了 Service 函数里。

即新版本中，驱动的指定代码为：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
 
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")

driver = webdriver.Chrome(service=service)
driver.get('https://www.baidu.com')
driver.quit()
```
