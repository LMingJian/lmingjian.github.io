---
title: 如何使用 Selenium 获取 input 标签的值
date: 2024-12-25T16:13:25+08:00
author: LiangMingJian
---

# 需求

在定位 input 标签后，获取标签的值。

# 通过 `get_attribute('value')` 获取值

```python
inputContext1 = driver.find_element_by_xpath('//input').get_attribute('value')
```

# 通过 `get_attribute('textContent')` 获取值

```python
logoContext1 = driver.find_element_by_xpath('//div[@class="logo"]/span').get_attribute('textContent')
```

# 通过 `text` 属性获取值

```python
logoContext2 = driver.find_element_by_xpath('//div[@class="logo"]/span').text
```

# 通过 JS 的 `value` 属性获取值

```python
inputContext2 = "return document.getElementsByClassName('ivu-input')[0].value"
driver.execute_script(inputContext2)
```

# 通过 JS 的 `innerText` 属性获取值

```python
logoContext3 = "return document.getElementsByTagName('span')[0].innerText"
driver.execute_script(inputContext3)
```
