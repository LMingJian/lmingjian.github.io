---
title: Selenium 远程驱动
date: 2023-09-05
author: LM
---

## 1.先决条件

要支持使用远程 WebDriver，目标系统必须满足以下条件。

- 需要安装 Java 11 或更高版本
- 需要安装浏览器
- 需要安装浏览器驱动程序（目标系统需要已经安装并配置了 PATH 环境变量）

## 2.下载并启动 Selenium Grid 服务

- 从[最新的发布版本](https://github.com/SeleniumHQ/selenium/releases/latest)下载 Selenium Server jar 文件。
- 执行 `java -jar selenium-server-<version>.jar standalone`，启动 Grid。

## 3.连接服务

- 将您的 WebDriver 测试指向 http://localhost:4444 。
- 可以通过在浏览器中打开 http://localhost:4444 来检查正在运行的测试和可用的功能。

```python
from selenium import webdriver

firefox_options = webdriver.FirefoxOptions()
driver = webdriver.Remote(
    command_executor='http://www.example.com',
    options=firefox_options
)
driver.get("http://www.google.com")
driver.quit() 
```