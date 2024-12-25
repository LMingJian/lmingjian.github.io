---
title: Selenium 的使用
date: 2024-12-25T16:13:47+08:00
author: LiangMingJian
---

# 概述

Selenium 是一个用于测试网站的自动化测试工具，支持各种浏览器，如 Chrome、Firefox、Edge 等，可使用以下命令进行安装。

```
pip install Selenium
```

# 环境配置

Selenium 的使用是基于浏览器驱动 webdriver 来启动与操作网页的，因此想要正常的进行 Selenium 自动化测试就必须获取对应浏览器的 webdriver 驱动文件。

可以在环境变量中配置 Path 的属性，令其指向放置 webdriver 驱动文件的文件夹，或在程序代码中指定 webdriver 驱动文件的位置。

```
chromedriver.exe --version
geckodriver.exe --version
```

# 元素的定位

```
|元素               | 定位一个元素                       | 含义                 
|-------------------| --------------------------------- | -----------------
|id                 | find_element_by_id                | 通过 id 定位
|name               | find_element_by_name              | 通过 name 定位    
|xpath              | find_element_by_xpath             | 通过 xpath 定位  
|link_texr          | find_element_by_link_text         | 通过完整超链接定位   
|partial_link_text  | find_element_by_partial_link_text | 通过部分链接定位
|tag_name           | find_element_by_tag_name          | 通过标签定位
|class_name         | find_element_by_class_name        | 通过类名进行定位
|css_selector       | find_elements_by_css_selector     | 通过 css 进行定位 
```

对象的定位是自动化测试的核心，要想操作一个对象，首先应该识别这个对象。一个对象就像一个人一样，他会有各种的特征（属性），我们可以通过这个属性找到这对象。以百度搜索的输入框为例，有以下属性（F12 查看）。

```html
<input id="kw" class="s_ipt" name="wd" value="" maxlength="255" autocomplete="off"/>
```

我们可以这样来定位属性：

```python
# 通过 id 方式定位
browser.find_element_by_id("kw")
# 通过 name 方式定位
browser.find_element_by_name("wd")
# 通过 tag name 方式定位
browser.find_element_by_tag_name("input")
# 通过 class name 方式定位
browser.find_element_by_class_name("s_ipt")
# 通过 CSS 方式定位
browser.find_element_by_css_selector("#kw")
browser.find_element_by_css_selector("html > body > form > span > input")
# 通过 xpath 方式定位
browser.find_element_by_xpath("//input[@id='kw']")
browser.find_element_by_xpath("/html/body/form/span/input")
```

复数元素的定位，只要在 `find_element` 后面加上 `s` 便可，此时返回的是元素的列表。

```python
elements = driver.find_elements_by_xpath('//div/h3/a')
```

# 浏览器的基本操控

```
| 方法                | 说明                  
| ------------------- | ----------------------
| set_window_size()   | 设置浏览器的大小       
| back()              | 控制浏览器后退         
| forward()           | 控制浏览器前进         
| refresh()           | 刷新当前页面           
| clear()             | 清除文本               
| send_keys (value)   | 模拟按键输入           
| click()             | 单击元素               
| submit()            | 用于提交表单           
| get_attribute(name) | 获取元素属性值         
| is_displayed()      | 设置该元素是否用户可见 
| size                | 返回元素的尺寸         
| text                | 获取元素的文本         
```

使用举例：

```python
from selenium import webdriver
# 1.创建浏览器对象
browser = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
# 2.通过浏览器向服务器发送URL请求
browser.get("https://www.baidu.com/")
# 3.刷新浏览器
browser.refresh()
# 4.设置浏览器的大小
browser.set_window_size(1400,800)
# 5.设置链接内容
element=browser.find_element_by_link_text("新闻")
element.click()
```

# 鼠标的模拟操作

```
| 方法                   | 说明    
| ---------------------- | -------------------------
| ActionChains(driver)   | 构造 ActionChains 对象 
| context_click()        | 右击    
| move_to_element(above) | 执行鼠标悬停操作 
| double_click()         | 双击        
| drag_and_drop()        | 拖动     
| move_to_element(above) | 执行鼠标悬停操作   
| context_click()        | 用于模拟鼠标右键操作， 在调用时需要指定元素定位 
| perform()              | 执行所有 ActionChains 中存储的行为, 可以理解成是对整个操作的提交动作 
```

使用举例：

```python
from selenium import webdriver
# 1.引入 ActionChains 类
from selenium.webdriver.common.action_chains import ActionChains
driver = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
driver.get("https://www.baidu.com")
# 2.定位到要悬停的元素
element= driver.find_element_by_link_text("设置")
# 3.对定位到的元素执行鼠标悬停操作
ActionChains(driver).move_to_element(element).perform()
# 4.寻找链接
elem1=driver.find_element_by_link_text("搜索设置")
elem1.click()
```

# 键盘的模拟操作

```
| 模拟键盘按键               | 说明                
| -------------------------- | ------------------- 
| send_keys(Keys.BACK_SPACE) | 删除键（BackSpace） 
| send_keys(Keys.SPACE)      | 空格键（Space）       
| send_keys(Keys.TAB)        | 制表键（Tab）         
| send_keys(Keys.ESCAPE)     | 回退键（Esc）       
| send_keys(Keys.ENTER)      | 回车键（Enter）     
| send_keys(Keys.CONTROL,'a') | 全选（Ctrl+A） 
| send_keys(Keys.CONTROL,'c') | 复制（Ctrl+C） 
| send_keys(Keys.CONTROL,'x') | 剪切（Ctrl+X） 
| send_keys(Keys.CONTROL,'y') | 粘贴（Ctrl+V） 
| send_keys(Keys.F1…Fn)       | 键盘（F1…Fn）     
```

使用举例：

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
browser = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
browser.get("https://www.baidu.com")
elem=browser.find_element_by_id("kw")
elem.send_keys('Selenium')
elem.send_keys(Keys.ENTER)
```

# 断言

我们可以采用以下属性作为断言参考。

```
| 属性        | 说明                   
| ----------- | ----------------------
| title       | 用于获得当前页面的标题 
| current_url | 用户获得当前页面的 URL  
| text        | 获取搜索条目的文本信息 
```

# 表单的切换

```
| 方法                        | 说明 
| --------------------------- | --------------------
| switch_to.frame()           | 将当前定位的主体切换为 frame/iframe 表单的内嵌页面中
| switch_to.default_content() | 跳回最外层的页面                               
```

# 窗口的切换

```
| 方法                  | 说明 
| --------------------- | ----------------------
| current_window_handle | 获得当前窗口句柄     
| window_handles        | 返回所有窗口的句柄到当前会话   
| switch_to.window()    | 用于切换到相应的窗口
```

# 警告框的处理

```
| 方法                  | 说明                                               
| --------------------- | ------------------------------
| text                  | 返回 alert/confirm/prompt 中的文字信息             
| accept()              | 接受现有警告框                                     
| dismiss()             | 解散现有警告框                                     
| send_keys()           | 发送文本至警告框 
| switch_to_alert       | 切换到 alert
```

# 下拉框的选择

```
from selenium.webdriver.support.select import Select
| 方法                              | 说明                      
| ---------------------------------| ------------------------- 
| select_by_value("选择值")         | select 标签的 value 属性的值 
| select_by_index("索引值")         | 下拉框的索引              
| select_by_visible_testx("文本值") | 下拉框的文本值           
```

# cookie 的操作

```
| 方法                              | 说明  
| --------------------------------- | -----------------
| get_cookies()                     | 获得所有 cookie 信息 
| get_cookie(name)                  | 返回字典的 key 为 “name” 的 cookie 信息
| add_cookie(cookie_dict)           | 添加 cookie，“cookie_dict” 指字典对象，必须有 name 和 value 值 
| delete_cookie(name,optionsString) | 删除 cookie 信息，“name” 是要删除的 cookie 的名称，“optionsString” 是该 cookie 的选项
| delete_all_cookies()              | 删除所有 cookie 信息 
```

# 窗口截图

```
| 方法                                   | 说明                               
| -------------------------------------- | -----------
| get_screenshot_as_file(self, filename) | 用于截取当前窗口, 并把图片保存到本地 
```

# 关闭浏览器

```
| 方法    | 说明         
| ------- | ------------ 
| close() | 关闭单个窗口 
| quit()  | 关闭所有窗口 
```

# 获取网站源码

```
| 方法    | 说明         
| ------- | ------------ 
| page_source | 获取网站源码 
```
