---
title: Selenium 在使用 executable_path 时报错
date: 2021-11-20
author: LMingJian
---

## BUG 描述

Selenium 在使用 executable_path 时，程序能正常运行，但会出现 DeprecationWarning 警告的类型错误。

## Resolution

这是由于版本更新时，所使用的方法过时的原因，表示该函数在当前版本被重构，还可以传入参数，但是在之后的某个版本会被删除。（官方：Executable path has been deprecated please pass in a Service object in Selenium Python）。查询当前版本重构后的函数，是因为之前的 executable_path 被重构到了 Service 函数里。

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
 
s = Service(r"D:\Software\webdrivers\chromedriver.exe")
driver = webdriver.Chrome(service=s)
driver.get('https://www.baidu.com')
driver.close()
```

