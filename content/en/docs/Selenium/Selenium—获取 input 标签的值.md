---
title: Selenium 实现 input 标签值的获取
date: 2021-03-26
author: LM
---

## 1.需求

在定位 input 标签后，获取标签的值。

## 2.实现：通过 `get_attribute('value')` 获取值

```python
inputContext1 = driver.find_element_by_xpath('//input').get_attribute('value')
```

## 3.实现： 通过 `get_attribute('textContent')` 来获取值

```python
logoContext1 = driver.find_element_by_xpath('//div[@class="logo"]/span').get_attribute('textContent')
```

## 4.实现： 通过 `text` 属性来获取值

```python
logoContext2 = driver.find_element_by_xpath('//div[@class="logo"]/span').text
```

## 5.实现： 通过 Js 的 `value` 属性来获取值

```python
inputContext2 = "return document.getElementsByClassName('ivu-input')[0].value"
driver.execute_script(inputContext2)
```

## 6.实现： 通过 Js 的 `innerText` 属性来获取值

```python
logoContext3 = "return document.getElementsByTagName('span')[0].innerText"
driver.execute_script(inputContext3)
```