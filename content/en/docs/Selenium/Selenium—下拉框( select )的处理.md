---
title: Selenium 实现对<select>标签的处理
date: 2021-07-29
author: LM
---

## 1.需求

实现 HTML 中 `<select>` 标签的自动化选择。

## 2.实现

```python
from selenium.webdriver.support.select import Select

select = driver.find_element_by_name('')
# 根据下标选择，从0开始
Select(select).select_by_index(1)
time.sleep(2)
# 根据value的值选择
Select(select).select_by_value('b')
time.sleep(2)
# 根据text选择
Select(select).select_by_visible_text('AA')
time.sleep(2)
# 断言
select = Select(self.driver.find_element_by_xpath(''))  # 定位下拉框
print(select.first_selected_option.text)  # 获取下拉框当前选中得值
select.select_by_value('b')  # 选择下拉框得另一个选项
time.sleep(1)
print(select.first_selected_option.text)  # 重新获取下拉框选择的值
```

## 3.实现：其他功能

| 方法                       | 说明                   |
| -------------------------- | ---------------------- |
| select_by_index()          | 通过索引定位           |
| select_by_value()          | 通过value值定位        |
| select_by_visible_text()   | 通过文本值定位         |
| deselect_all()             | 取消所有选项           |
| deselect_by_index()        | 取消对应index选项      |
| deselect_by_value()        | 取消对应value选项      |
| deselect_by_visible_text() | 取消对应文本选项       |
| first_selected_option()    | 返回第一个选项         |
| all_selected_options()     | 返回所有的选项         |
| options()                  | 返回所以的选择项       |
| all_selected_options()     | 返回所以已选中的选择项 |
| first_selected_option()    | 返回选中的第一个选择项 |

