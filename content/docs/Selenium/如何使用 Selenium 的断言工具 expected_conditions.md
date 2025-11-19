---
title: 如何使用 Selenium 的断言工具 expected_conditions
date: 2024-12-25T16:13:34+08:00
author: LiangMingJian
---

# 概述

expected_conditions 是 Selenium 提供的断言工具，它提供一系列预定义条件来判断页面状态是否符合预期。

expected_conditions 一般不会单独使用，基本上都是与 WebDriverWait 配合使用，进行显式等待，动态等待页面上元素显示。

> WebDriverWait 显式等待的介绍，请看文章：[如何在 Selenium 中进行等待](https://zhuanlan.zhihu.com/p/1972616600042578872)

> 另外对于部分的断言条件（返回值是 True 或 False 的），expected_conditions 支持传入一个 webdriver 参数来直接打印布尔值。

# 判断标题

- `title_is()`：判断当前页面的 title 是否完全等于预期，返回 True 或 False
- `title_contains()`：判断当前页面的 title 是否包含预期字符，返回 True 或 False

```python
from selenium import webdriver  
from selenium.webdriver.support import expected_conditions as EC  
from selenium.webdriver.support.wait import WebDriverWait  
  
service = webdriver.ChromeService(r"F:\Sdk\webdriver\chromedriver.exe")  
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("https://www.baidu.com")  
  
print(WebDriverWait(driver, 2).until(EC.title_is('百度一下，你就知道')))
# True
print(WebDriverWait(driver, 2).until(EC.title_contains('百度')))  
# True

# 对于仅返回布尔值的 EC，可以直接传入 webdriver 来打印对于的断言结果
print(EC.title_is('百度一下，你就知道')(driver))
# True
  
driver.quit()
```

# 判断元素是否加载

- `presence_of_element_located()`：判断元素是否已经加载并返回 True 或 False
- `presence_of_all_elements_located()`：判断是否至少有 1 个元素已经加载并返回所有 WebElement 对象的**列表**

> 注意，这里的加载只是代表元素已经出现在 HTML DOM 树中，并不指元素可见。

> 另外这里返回的是元素对象，因此可以直接操作。

```python
from selenium import webdriver  
from selenium.webdriver.common.by import By  
from selenium.webdriver.support import expected_conditions as EC  
from selenium.webdriver.support.wait import WebDriverWait  
  
service = webdriver.ChromeService(r"F:\Sdk\webdriver\chromedriver.exe")  
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("https://www.baidu.com")  
  
element = WebDriverWait(driver, 2).until(  
    EC.presence_of_element_located((By.ID, "kw")))  
print(element.get_attribute("name"))  
# wd
  
element = WebDriverWait(driver, 2).until(  
    EC.presence_of_all_elements_located((By.ID, "kw")))  
print(element[0].get_attribute("name"))
# wd
  
driver.quit()
```

# 判断元素是否可见

- `visibility_of_element_located()`：判断一个元素是否已加载，可见，返回一个 WebElement 对象
- `visibility_of_any_elements_located()`：判断是否至少有 1 个元素已加载，可见，返回所有 WebElement 对象的列表
- `element_to_be_clickable()`：判断一个元素是否已加载，可见，enable 可操作，返回一个 WebElement 对象
- `visibility_of()`：判断**一个已加载的元素**是否可见并返回 WebElement 对象

> 注意，这里的可见是指元素的宽和高都不为 0，如果宽或高有一个为 0 都属于不可见，一般来说，不可见的元素是无法操作的。

> 百度的搜索栏 input 标签是不可见的，高为 0，其输入是通过其他元素写入的。

```python
from selenium import webdriver  
from selenium.webdriver.common.by import By  
from selenium.webdriver.support import expected_conditions as EC  
from selenium.webdriver.support.wait import WebDriverWait  
  
service = webdriver.ChromeService(r"F:\Sdk\webdriver\chromedriver.exe")  
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("https://www.baidu.com")  
  
element = WebDriverWait(driver, 2).until(  
    EC.visibility_of_element_located((By.ID, "chat-submit-button")))  
print(element.text)     # 百度一下
  
element = WebDriverWait(driver, 2).until(  
    EC.visibility_of_any_elements_located((By.ID, "chat-submit-button")))  
print(element[0].text)  # 百度一下

element = WebDriverWait(driver, 2).until(  
    EC.element_to_be_clickable((By.ID, "chat-submit-button")))  
print(element.text)  # 百度一下

# ------------

element = driver.find_element(By.ID, "chat-submit-button")    
element = WebDriverWait(driver, 2).until(  
    EC.visibility_of(element))    
print(element.text)   # 百度一下

# 下述元素是存在的，但因为不可见所以返回 False
kw1 = driver.find_element(By.ID, "kw")  
kw2 = EC.visibility_of(kw1)(driver)  # 这是 EC 另一种调用方法
print(kw2)  # False

driver.quit()
```

# 判断元素的属性

- `text_to_be_present_in_element()`：判断元素的 text 属性是否包含预期值，返回一个 bool 对象
- `text_to_be_present_in_element_value()`：判断元素的 value 属性是否包含预期值，返回一个 bool 对象
- `text_to_be_present_in_element_attribute()`：判断元素的 attribute 属性是否包含预期值，返回一个 bool 对象

```python
from selenium import webdriver  
from selenium.webdriver.common.by import By  
from selenium.webdriver.support import expected_conditions as EC  
  
service = webdriver.ChromeService(r"F:\Sdk\webdriver\chromedriver.exe")  
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("https://www.baidu.com")  

print(EC.text_to_be_present_in_element((By.ID, "chat-submit-button"), '百度')(driver))  
# True
print(EC.text_to_be_present_in_element_value((By.ID, "chat-submit-button"), '百度')(driver))  
# False
print(EC.text_to_be_present_in_element_attribute((By.ID, "chat-submit-button"), 'name', '1')(driver))  
# False

driver.quit()
```

# 判断元素的选中状态

这部分内容，一般是用于下拉表单的选择，用来检查选项是否被选中。

- `element_to_be_selected()`：判断元素是否被选中，返回一个 bool 值，传入的是一个 WebElement 对象。
- `element_selection_state_to_be()`：判断元素的选中状态是否符合预期，返回一个 bool 值，传入的是一个 WebElement 对象和一个 bool，用来判断是否满足选中还是未选中的状态。
- `element_located_selection_state_to_be()`：判断元素的选中状态是否符合预期，返回一个 bool 值，传入的是一个 `(By.ID, "xxx")` 位置元组和一个 bool，用来判断是否满足选中还是未选中的状态。

# 其他判断

- `frame_to_be_available_and_switch_to_it()`：判断该 frame 是否可以切换进去 ，返回一个 bool 值，传入可以是一个 `(By.ID, "xxx")` 位置元组，也可以是一个 WebElement 对象。
- `alert_is_present()`：判断页面上是否存在 alert 弹窗，返回一个 Alert 对象（用于操作 弹窗的 WebElement 对象），没有传入参数。
- `invisibility_of_element_located()`：判断某个元素是否不存在于 DOM 树或不可见，返回一个 bool 值，传入可以是一个 `(By.ID, "xxx")` 位置元组，也可以是一个 WebElement 对象。
- `staleness_of()`：判断某个元素是否从 DOM 树中移除，传入一个 WebElement 对象，返回一个 bool 值。
