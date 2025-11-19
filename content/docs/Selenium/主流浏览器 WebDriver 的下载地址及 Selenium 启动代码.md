---
title: 主流浏览器 WebDriver 的下载地址及 Selenium 启动代码
date: 2025-11-11T11:58:46+08:00
author: LiangMingJian
---

# Chrome

**谷歌浏览器，chromedriver**

官方地址：[ 什么是 ChromeDriver？](https://developer.chrome.google.cn/docs/chromedriver?hl=zh-cn)

![](_images/drawingbed/img/Pasted%20image%2020251111120245.png)

下载地址：[ Chrome for Testing ](https://googlechromelabs.github.io/chrome-for-testing/)

![](_images/drawingbed/img/Pasted%20image%2020251111120231.png)

Selenium 启用：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import time

service = Service(r"F:\Sdk\webdriver\chromedriver.exe")

driver = webdriver.Chrome(service=service)
driver.maximize_window()
driver.get("https://www.bing.com/")

time.sleep(3)
```

# Edge

**边缘浏览器，msedgedriver**

官方地址：[ Microsoft 边缘网络驱动程序 ](https://developer.microsoft.com/zh-cn/microsoft-edge/tools/webdriver/?form=MA13LH)

下载地址：同上

![](_images/drawingbed/img/Pasted%20image%2020251111135937.png)

![](_images/drawingbed/img/Pasted%20image%2020251111135949.png)

Selenium 启用：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
import time

service = Service(r"F:\Sdk\webdriver\msedgedriver.exe")

driver = webdriver.Edge(service=service)
driver.maximize_window()
driver.get("https://www.bing.com/")

time.sleep(3)
```

# Firefox

**火狐浏览器，geckodriver**

官方地址：[ mozilla / geckodriver ](https://github.com/mozilla/geckodriver)

![](_images/drawingbed/img/Pasted%20image%2020251111140533.png)

下载地址：[ mozilla / geckodriver / releases ](https://github.com/mozilla/geckodriver/releases)

![](_images/drawingbed/img/Pasted%20image%2020251111141129.png)

镜像地址：[ 华为云镜像 ](https://mirrors.huaweicloud.com/geckodriver/)

![](_images/drawingbed/img/Pasted%20image%2020251111140648.png)

Selenium 启用：

> 特别注意，在使用 Firefox 时，需要用 FirefoxService 指定驱动，而不能使用 Service。

> 在使用 Service 时，Selenium 不支持自动查找 Firefox 驱动，程序会报错：`selenium.common.exceptions.WebDriverException: Message: Service F:\Sdk\webdriver\geckodriver.exe unexpectedly exited. Status code was: 64`

```python
from selenium import webdriver
import time
  
service = webdriver.FirefoxService(r"F:\Sdk\webdriver\geckodriver.exe")

driver = webdriver.Firefox(service=service)
driver.maximize_window()
driver.get("https://www.bing.com/")

time.sleep(3)
```