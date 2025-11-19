---
title: 如何使用 Selenium 处理 select 标签（下拉表单）
date: 2024-12-25T16:13:20+08:00
author: LiangMingJian
---

# 需求

实现 HTML 中 `<select>` 标签（下拉表单）的自动化选择。

![](_images/drawingbed/img/Pasted%20image%2020251111114438.png)

```html
<!-- test.html -->
<label for="pet-select">Choose a pet:</label>

<select name="pets" id="pet-select">
  <option value="">--Please choose an option--</option>
  <option value="dog">Dog</option>
  <option value="cat">Cat</option>
  <option value="hamster">Hamster</option>
  <option value="parrot">Parrot</option>
  <option value="spider">Spider</option>
  <option value="goldfish">Goldfish</option>
</select>
```

> 构造以上的 HTML 代码文件，用于后文使用。

# 代码

在 Selenium 中，提供工具类 Select 用于自动化 `<select>` 标签的选择。

> 特别注意，该工具类只能用于处理 `<select>` 标签，现代网页中有部分类似的下拉表单实际可能不是 `<select>` 标签，这些需要自行另外处理。

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.select import Select
import time

# 定位驱动
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")

# 启动浏览器
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("file:///F:/MyPython/PythonProject313/test.html")  

# 定位 <select> 标签，然后实例化工具类
element = driver.find_element(By.ID, "pet-select")  
select = Select(element)
```

在实例化 Select 类后，我们便可以使用 Select 所提供的方法来完成对下拉表单选项的选择。

```python
# 根据索引选择元素，索引从 0 开始
select.select_by_index(0)
# 打印第一个选择项
print(select.first_selected_option.text)
# 打印：--Please choose an option--
time.sleep(2)

select.select_by_index(1)
print(select.first_selected_option.text)
# 打印：Dog
time.sleep(2)

# 通过 value 值定位也是可以的
select.select_by_value("cat")  
print(select.first_selected_option.text)  
# Cat
time.sleep(2)  

# 通过文本值定位也行
select.select_by_visible_text("Hamster")  
print(select.first_selected_option.text)  
# Hamster
time.sleep(2)
```

特别的，对于支持的多选 `<select>` 标签，工具类 Select 也适用。

> 要 `<select>` 标签支持多选，添加 multiple 属性即可

```python
select.select_by_index(1)  
select.select_by_index(2)  
# 多选时，需要使用 all_selected_options 方法返回所选择的 element 对象
for each in select.all_selected_options:  
    print(each.text)  
time.sleep(2)

# 多选时，支持取消选择
select.deselect_by_index(1)  
select.deselect_by_value('cat')  
select.deselect_by_visible_text("Cat")

# 支持一键取消所有选择
select.deselect_all()
```

# 附件：class Select

| 方法                       | 说明                  |
| -------------------------- | ---------------------|
| `select_by_index()`          | 通过索引定位           |
| `select_by_value()`        | 通过 value 值定位        |
| `select_by_visible_text()`   | 通过文本值定位         |
| `deselect_all()`             | 取消所有选项           |
| `deselect_by_index()`        | 取消对应索引选项      |
| `deselect_by_value() `       | 取消对应 value 值选项      |
| `deselect_by_visible_text()` | 取消对应文本值选项       |
| `first_selected_option()`    | 返回第一个已选择的选项         |
| `all_selected_options()`     | 返回所有已选择的选项         |
| `options()`                  | 返回所有的选择项       |
| `is_multiple`                  | 返回布尔值，是否多选       |
