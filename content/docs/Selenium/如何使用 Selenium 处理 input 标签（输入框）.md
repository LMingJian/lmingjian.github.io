---
title: 如何使用 Selenium 获取 input 标签的值
date: 2024-12-25T16:13:25+08:00
author: LiangMingJian
---

# 需求

在定位 `<input>` 标签后，完成数据输入和数据获取的工作。

![](_images/drawingbed/img/Pasted%20image%2020251112141442.png)

```html
<!-- test.html -->
<label for="name">Name (4 to 8 characters):</label>  
  
<input  
  type="text"  
  id="name"  
  name="name"  
  required  
  minlength="4"  
  maxlength="8"  
  size="10" />
```

> 构造以上的 HTML 代码文件，用于后文使用。

# 代码

**输入数据：**

```python
from time import sleep  
from selenium import webdriver  
from selenium.webdriver.chrome.service import Service  
from selenium.webdriver.common.by import By  
  
# 定位驱动
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")  

# 启动浏览器
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("file:///F:/MyPython/PythonProject313/test.html")  

# 定位元素
element = driver.find_element(By.ID, "name")  

# 输入数据
element.send_keys('Hello')  
sleep(2)
```

**获取数据：**

在 Selenium 中，获取一个标签数据的功能实现大致上可以分为以下几种逻辑：

- 通过 `get_attribute()` 方法，读取标签所具有的 HTML 属性值来获取数据（**一般推荐使用该方法**）。
- 通过读取 Selenium 捕获元素的 `text` 属性来获取数据（特别注意，这个方法仅适用于具备 input 属性的 div 标签，因为 input 标签的输入内容存储在 value 属性中而非作为元素文本内容存在） 。
- 通过执行 `JavaScript` 代码来读取标签的属性值来获取数据。

对于上述三种逻辑，在使用时，应当根据实际环境进行选择使用，上述方法对应的代码实现可以是以下内容：

```python
# 读取 value 属性
text = element.get_attribute('value')  
print(text)

# 通过 text 属性获取（对于标准的 input 标签，这个是没有数据的）
text = element.text 
print(text)

# 通过 js 获取数据
js = "return document.getElementById('name').value"  
text = driver.execute_script(js)  
print(text)
```
