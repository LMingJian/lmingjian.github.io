---
title: 如何使用 Selenium 捕获浏览器 F12 控制台信息
date: 2024-12-25T16:13:30+08:00
author: LiangMingJian
---

# 需求

在执行 Selenium 自动化脚本时，需要捕获浏览器 F12 开发者工具中控制台的信息。

# 实现

Selenium webdriver 提供方法 `get_log('browser')` 用来获取浏览器日志。

需要注意的是，对于大部分浏览器，日志记录功能一般不会默认开启。因此，在使用 Selenium 捕获日志时，需要先通过 `options.set_capability()` 方法，配置浏览器的能力参数，使其能够记录日志。

```python
from selenium import webdriver  
import time  
  
service = webdriver.ChromeService(r"F:\Sdk\webdriver\chromedriver.exe")  
  
# 配置浏览器选项并开启日志捕获  
chrome_options = webdriver.ChromeOptions()  
chrome_options.set_capability('goog:loggingPrefs', {'browser': 'ALL'})  
  
driver = webdriver.Chrome(service=service, options=chrome_options)  
  
driver.maximize_window()  
driver.get("https://www.baidu.com")  
time.sleep(2)  
  
# 捕获日志  
browser_logs = driver.get_log('browser')  
print(browser_logs)  
  
time.sleep(10)  
driver.quit()
```

> 需要注意，上述代码在测试后发现仅对于 Chrome 有用，对于 Edge 或 Firefox 则无法生效。如果有相关修改建议，请在文章评论告知，或等待后续更新。

# 拓展

对于参数 `goog:loggingPrefs`，除了支持捕获 `browser` 浏览器日志外，还支持捕获 `performance` 性能日志，`driver` 驱动日志等内容。

```python
...
chrome_options.set_capability(  
    'goog:loggingPrefs',  
    {  
        'browser': 'ALL',  
        'performance': 'ALL',  
        'driver': 'ALL'  
    }  
)
...
# 打印捕获的日志类型
print(driver.log_types)
# 浏览器日志
browser_logs = driver.get_log('browser')  
print(browser_logs)  
# 性能日志
performance_logs = driver.get_log('performance')  
print(performance_logs)  
# 驱动日志
driver_logs = driver.get_log('driver')  
print(driver_logs)
```
