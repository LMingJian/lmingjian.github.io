---
title: 如何使用 Selenium 远程操作浏览器
date: 2024-12-25T16:13:23+08:00
author: LiangMingJian
---

# 先决条件

要支持使用远程 WebDriver，远端目标系统必须满足以下条件。

- 需要安装 Java 11 或更高版本（[ Java 下载 ](https://www.oracle.com/cn/java/technologies/downloads/)）
- 需要安装浏览器
- 需要安装浏览器驱动程序（**WebDriver 驱动必须配置在 PATH 环境变量中**）

# 下载并启动 Selenium Grid 服务

在准备好环境后，便可以下载安装并启动 Selenium Grid 服务。

- 从 [ Selenium Grid 项目地址 ](https://github.com/SeleniumHQ/selenium/releases/latest) 下载最新的 selenium-server.jar 文件。
- 随后在远程电脑中执行 `java -jar selenium-server.jar standalone` 命令，启动 Selenium Grid 服务。
- 在启动服务后，可以通过 http://localhost:4444 端口查看服务运行情况，对于其他电脑，可以使用 http://远端系统IP:4444 来进行访问。

![](_images/drawingbed/img/Pasted%20image%2020251112170114.png)

![](_images/drawingbed/img/Pasted%20image%2020251112170557.png)

# 连接服务

最后，通过使用 `webdriver.Remote()` 方法，可以远程连接 Selenium Grid 服务，实现远程浏览器自动化。

特别注意，在远程连接时，如果远程电脑中存在多个浏览器，则用户需要使用 `webdriver.ChromeOptions()` 来对浏览器进行指定。

- Chrome：`webdriver.ChromeOptions()`
- Edge：`webdriver.EdgeOptions()`
- Firefox：`webdriver.FirefoxOptions()`

```python
import time  
from selenium import webdriver  

# 指定并启动远程浏览器
chrome_options = webdriver.ChromeOptions()  
driver = webdriver.Remote(  
    command_executor='http://localhost:4444',  
    options=chrome_options  
)  

# 操作浏览器
driver.maximize_window()  
driver.get("https://cn.bing.com")  
time.sleep(3)  
driver.quit()
```

————————————

> [ Selenium Grid 文档 ](https://www.selenium.dev/zh-cn/documentation/grid/)
