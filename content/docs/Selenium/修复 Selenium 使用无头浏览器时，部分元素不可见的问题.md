---
title: 修复 Selenium 使用无头浏览器时，部分元素不可见的问题
date: 2024-12-25T16:13:39+08:00
author: LiangMingJian
---

# BUG 描述

Selenium WebDriver 在使用 headless 无头模式时，默认会将窗口设置为 0x0，并且处于最小化 Minimized 状态。因此在程序启动后，部分元素会因为超出界面而不可见，进而导致出现无法点击的异常。

# Resolution

在启动 Selenium WebDriver 时先配置浏览器大小，或在启动浏览器后执行最大化方法。

```python
from selenium import webdriver  
from selenium.webdriver.chrome.service import Service  
import time  
  
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")  
options = webdriver.ChromeOptions()  
options.add_argument('--headless')  
# 通过配置启动参数来默认最大化
options.add_argument('--start-maximized')  
  
driver = webdriver.Chrome(service=service, options=options)  
# 当然，也可以在启动后最大化
driver.maximize_window()  
driver.get("https://www.bing.com/")  
  
time.sleep(3)
```
