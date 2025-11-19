---
title: Selenium 的使用手册
date: 2024-12-25T16:13:47+08:00
author: LiangMingJian
---

# 概述

Selenium 是一个用于网站自动化测试的工具，其通过 WebDriver 来操作各种浏览器，如 Chrome、Firefox、Edge 等。

Python 环境中可使用以下命令进行安装：

```python
pip install Selenium
```

# 环境配置

Selenium 的使用是基于浏览器驱动 WebDriver 来启动与操作网页的，因此想要正常的进行 Selenium 自动化测试就必须获取对应浏览器的 WebDriver 驱动文件。

开发者可以在环境变量中配置 WebDriver 的 Path 属性，令其指向放置 WebDriver 驱动文件的文件夹，或是程序代码中指定 WebDriver 驱动文件的位置。

比如下面启动 Chrome 浏览器的 Selenium 代码：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service

# 指定驱动位置，如果驱动在环境变量中，则该参数可以忽略
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")

driver = webdriver.Chrome(service=service)  # 打开自动化浏览器
driver.maximize_window()             # 最大化浏览器
driver.get("https://www.bing.com/")  # 访问指定网页
```

# 浏览器的操控

Selenium 通过 WebDriver 可以操控浏览器完成多样的工作，除了上述使用到的最大化和访问网页操作外，还支持下述功能：

| 方法                               | 说明                  
| -------------------           | ----------------------   
| `back()`                       | 控制浏览器后退         
| `forward()`                 | 控制浏览器前进         
| `refresh()`                | 刷新当前页面 
| `close()`                   | 关闭当前页面 
| `quit()`                     | 退出浏览器 
| `execute_script()`    | 执行 Javascript 脚本
| `add_cookie()`           | 添加 Cookie 
| `page_source`           |  获取网站源码 
| `title`                   | 获得当前页面的标题 
| `current_url`      | 获取当前页面的 URL  

上面的方法需要通过 `webdriver.Chrome()` 实例化的浏览器对象来调用，这些方法只是 Selenium 对浏览器操控的一部分，更多的方法可以在 Selenium 代码中查看。

```python
...
driver = webdriver.Chrome()
driver.refresh()
```

# 元素的定位

元素的定位是自动化测试的核心，要想操作一个元素，首先就要识别这个元素。

HTML 网页的一个元素就像一个人一样，会有各种的特征属性，我们可以通过这些特征属性来找到对应的元素。

开发者在浏览器中按下 F12，然后选中元素，就可以看见如下所示的 HTML 源码。HTML 源码中的标签名，id，class，name，value 都是可以使用的特征属性。

```html
<input id="kw" class="s_ipt" name="wd" value="1"/>
```

在 Selenium 中，元素的查找定位需要使用查找方法 `find_element()` 和定位模块 `By` 组合使用。

定位模块 `By` 支持的定位方式有：

| 元素                   | 含义                 
| -------------------   | -----------------
|By.ID                  |  通过标签 id 属性定位
|By.NAME                |  通过标签 name 属性定位    
|By.XPATH               |  通过标签 xpath 属性定位  
|By.LINK_TEXT           |  通过完整超链接文本定位   
|By.PARTIAL_LINK_TEXT   |  通过部分链接文本定位
|By.TAG_NAME            |  通过标签名定位
|By.CLASS_NAME          |  通过类名进行定位
|By.CSS_SELECTOR        |  通过 CSS 选择器进行定位 

上面支持的定位方式中，通过 CSS 选择器 `By.CSS_SELECTOR` 是最常用的定位方式。

在选中定位方式后，我们可以使用方法 `find_element()` 进行元素定位：

```python
...
# 在定位模式后面需要跟随指定的特征属性
element = driver.find_element(By.CSS_SELECTOR, "#kw")
```

另外，如果元素存在多个，则对于**复数元素**的定位，只要在方法 `find_element` 后面加上 `s` 便可，此时，方法执行后返回的结果将会是元素的列表。

```python
...
elements = driver.find_elements(By.CSS_SELECTOR, "input")
```

# 元素的基本操控

在定位元素后，开发者便可以对元素进行操控，执行如点击，输入，获取等操作。

| 方法                               | 说明                  
| -------------------           | ----------------------                    
| `send_keys(value)`   | 模拟按键输入           
| `clear()`             | 清除文本     
| `click()`             | 单击元素                     
| `get_attribute(name)` | 获取元素属性值               
| `text`                | 获取元素的文本   

上述的方法需要对元素对象进行操作，同样展示的只是一部分方法，更多的方法可查阅 Selenium 代码。

```python
...
element = driver.find_element(By.CSS_SELECTOR, "#kw")
element.click()
element.send_keys("Hello")
element.clear()

print(element.get_attribute('value'))
print(element.text)
```

# 鼠标的模拟操作

Selenium 除了可以直接操作元素外，还可以通过构造类 `ActionChains` 来模拟用户鼠标操作，完成包括点击，双击，拖动等工作。

| 方法                   | 说明    
| ---------------------- | -------------------------
| `move_to_element()` | 移动鼠标到对应元素
| `click()`                     | 点击    
| `context_click()`        | 右击    
| `double_click()`        | 双击        
| `drag_and_drop()`        | 拖动     

需要注意的是，类 `ActionChains` 的执行不是实时执行的，它需要先写入操作步骤，然后通过类方法 `perform()` 提交动作执行。

示例代码如下：

```python
import time  
from selenium import webdriver  
from selenium.webdriver.chrome.service import Service  
from selenium.webdriver.common.by import By  
  
service = Service(r"F:\Sdk\webdriver\chromedriver.exe")  
  
driver = webdriver.Chrome(service=service)  
driver.maximize_window()  
driver.get("https://baidu.com")  
time.sleep(3)  

# 定位元素
element = driver.find_element(By.CSS_SELECTOR, "div.mnav a.s-bri")  
  
# 构造 ActionChains 类
action = webdriver.ActionChains(driver) 
# 写入操作
action.move_to_element(element)  
action.click(element) 
# 提交执行
action.perform()  
time.sleep(3)
```

# 键盘的模拟操作

除了模拟鼠标，Selenium 也支持模拟键盘操作。键盘的键值通过模块 Keys 进行维护，通过方法 `send_keys()` 进行发送。

| 模拟键盘按键               | 说明                
| -------------------------- | ------------------- 
| Keys.BACK_SPACE | 删除键（BackSpace） 
| Keys.SPACE    | 空格键（Space）       
| Keys.TAB      | 制表键（Tab）         
| Keys.ESCAPE    | 回退键（Esc）       
| Keys.ENTER   | 回车键（Enter）     
| Keys.CONTROL | 控制键（Ctrl） 
| Keys.F1      | （F1…Fn）  

方法 `send_keys()` 支持传入单个或多个 Keys 键值，传入多个 Keys 键值时，模拟使用的组合按键，比如 Ctrl+C 或 Ctrl+V。

另外这里展示的只是部分键值，更多的键值可查阅 Selenium 代码。

示例代码如下：

```python
...
element = driver.find_element(By.CSS_SELECTOR, "#chat-textarea")  
# 发送字符
element.send_keys('Selenium')  
# 发送组合快捷键
element.send_keys(Keys.CONTROL, 'V') 
# 发送单个快捷键
element.send_keys(Keys.ENTER)  
time.sleep(3)
```

# 内联页面的切换

在 HTML 网页中，存在 frame/iframe 这种内联页面，这种内联页面与外部页面是相互独立的，要操作这些内联页面的元素，需要让 WebDriver 驱动切换至该内联页面。

| 方法                        | 说明 
| --------------------------- | --------------------
| `switch_to.frame()`           | 将当前定位的主体切换为 frame/iframe 的内嵌页面中
| `switch_to.default_content()` | 跳回最外层的页面 

示例代码如下：

```python
...
# 定位 iframe 内联页面
element = driver.find_element(By.CSS_SELECTOR, "iframe") 
# 切换至该内联页面
driver.switch_to.frame(element)  
# 返回最外层页面
driver.switch_to.default_content()
```

# 警告框的处理

Selenium 支持对浏览器**原生的警告框**进行处理，通过 `driver.switch_to.alert` 方法，可以切换至警告框，然后进行处理。

| 方法                  | 说明                                               
| --------------------- | ------------------------------
| `text`                  | 返回 alert/confirm/prompt 中的文字信息             
| `accept()`              | 接受现有警告框                                     
| `dismiss()`             | 关闭现有警告框                                     
| `send_keys()`           | 发送文本至警告框 

示例代码如下：

```python
...
# 切换到弹窗
alert = driver.switch_to.alert
# 打印弹窗信息
print(alert.text)
# 点击确定
alert.accept() 
```

# Cookie 的操作

Selenium 支持对浏览器 Cookie 进行管理维护。

| 方法                              | 说明  
| --------------------------------- | -----------------
| `get_cookies()`                     | 获取所有 Cookie 信息 
| `get_cookie(name)`                | 返货 key 为 name 的 Cookie 信息
| `add_cookie(dict)` | 添加 Cookie，参数必须是 `{'name': 123, 'value': 123}` 的字典
| `delete_cookie(name)` | 删除 Cookie 信息，name 是要删除的 Cookie 名称
| `delete_all_cookies()`   | 删除所有 Cookie 信息 

示例代码如下：

```python
...
cookies = driver.get_cookies()
driver.add_cookie({'name': 'school', 'value': 1})
driver.delete_cookie('school')
```

# 窗口截图

| 方法                                   | 说明                               
| -------------------------------------- | -----------
| `get_screenshot_as_png()`  | 截取当前窗口为 PNG
| `get_screenshot_as_base64()`  | 截取当前窗口为 Base64
| `get_screenshot_as_file(filename)`  | 截取当前窗口为指定文件

```python
...
driver.get_screenshot_as_png()  
driver.get_screenshot_as_base64()  
driver.get_screenshot_as_file(filename="screenshot.png")
```

——————————

> [ Selenium 浏览器自动化项目文档 ](https://www.selenium.dev/zh-cn/documentation/)
