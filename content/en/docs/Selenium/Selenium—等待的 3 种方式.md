---
title: Selenium 的 3 种等待方式
date: 2024-06-19
author: LM
---

## 1.强制等待

直接使代码停止执行。

```python
import time

time.sleep(3)
```

## 2.隐式等待

设置一个全局隐性等待时间，如果在等待时间内找到了就执行下一步，未找到则报错。隐式等待在设置一次后，会对所有元素生效。需要注意的是，在使用隐式等待时，即使需要的某个元素已经出现了，仍然要等其他元素加载完毕，才能执行下一步，这是它的缺点。

```python
from selenium import webdriver
 
driver = webdriver.Chrome()
driver.get("https://www.jd.com")
driver.implicitly_wait(3)  # 设置隐式等待时间
driver.quit()
```

## 3.显式等待

每隔一段时间检查一下，条件成立则执行下一步，若到达设置的最长时间还未满足，则抛出超时异常。 需要注意的是，显示等待需要用到 WebDriverWait 类，配合该类的 `until()` 和 `until_not()` 方法来判断是否执行下一步，通常结合断言工具 expected_conditions （EC）来使用。

```python

from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("https://www.jd.com")
WebDriverWait(driver, 10,2).until(EC.presence_of_element_located((By.id, 'key')))
# 每隔 2s 判断元素是否出现，2 可以不设置，默认为 0.5s 轮询一次，最长 10s，超时未出现则报错
```

## 4.Ex.话题：页面的加载状态

Web 的页面属性 document.readyState 描述当前页面的加载状态，该属性有三个值：loading（正在加载）、interactive（可交互）、complete（完成）。默认情况下，在 document.readyState 为 complete 之前，WebDriver 都将延迟 `driver.get()` 的响应或 `driver.navigate().to()` 的调用。

对此，WebDriver 支持三种页面加载策略，通过属性 pageLoadStrategy 来描述，该属性有三种取值：normal（等待整个页面完全加载）、eager（待整个页面加载，但无关 CSS，图片）、none（仅等待至初始页面下载完成）。

默认情况下，当 Selenium WebDriver 加载页面时，遵循 normal 的页面加载策略，始终加载所有内容。但某些时候，你可以在页面访问缓慢时，停止下载其他资源，例如图片或 CSS。

```python
from selenium import webdriver

options = Options()
options.page_load_strategy = 'normal' # normal eager none 默认normal
driver = webdriver.Chrome(options=options)
driver.get("https://www.baidu.com")
print(driver.capabilities)
```

此外，你还可以使用 `set_page_load_timeout()` 主动设置加载超时时间，手动结束页面加载，然后执行代码。

```python
from selenium import webdriver
 
try:
    driver.set_page_load_timeout(10) # 设置页面加载时间，超时没完成则捕获异常
    driver.get("https://www.geeksforgeeks.org/")
except Exception as e:
    print(e)
    print("超时，直接进入下一步")
 
driver.find_element(By.XPATH, input).send_keys("python")
```

