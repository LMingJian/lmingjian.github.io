---
title: Selenium 自动化 Electron 软件
date: 2021-09-17
author: LM
---

## 1.需求

Electron 是一个基于 Node.js 和 Chromium 的开源框架，由于 Chromium 是谷歌浏览器的内核，因此我们可以把 Electron 看做是一个特别的浏览器。既然是浏览器，那显然可以使用 webdriver 对其进行控制，在 Python 中通过 selenium 实现对 Electron 的自动化测试。

## 2.实现

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
options.binary_location = "/Applications/Electron.app/Contents/MacOS/Electron"
driver = webdriver.Chrome(options=options, executable_path='chromedriver.exe')

# t = driver.find_elements_by_css_selector(".el-input__inner")
# t[0].send_keys('123456')
# t[1].send_keys('123456')
# sleep(3)
# driver.find_element_by_css_selector(".submit-item button").click()

driver.quit()
```

