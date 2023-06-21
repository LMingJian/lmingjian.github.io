---
title: Selenium 一种网页自动化工具
date: 2021-03-26
author: LM
---

## 1.简介

Selenium 是一个用于测试网站的自动化测试工具，支持各种浏览器，如 Chrome、Firefox、Edge 等。

```
pip install Selenium
```

## 2.环境

Selenium 的使用是基于浏览器驱动 webdriver 来启动与操作网页的，因此想要正常的进行 Selenium 自动化测试就必须获取对应浏览器的 webdriver 驱动文件。同时在环境变量中配置 Path 的属性，令其指向放置 webdriver 驱动文件的文件夹，或在程序中指定 webdriver 驱动文件的位置。

## 3.使用：元素的定位

```
|元素               | 定位一个元素                       | 含义                 
|-------------------| --------------------------------- | -------------------
|id                 | find_element_by_id                | 通过元素id定位        
|name               | find_element_by_name              | 通过元素name定位      
|xpath              | find_element_by_xpath             | 通过xpath表达式定位  
|link_texr          | find_element_by_link_text         | 通过完整超链接定位   
|partial_link_text  | find_element_by_partial_link_text | 通过部分链接定位      
|tag_name           | find_element_by_tag_name          | 通过标签定位          
|class_name         | find_element_by_class_name        | 通过类名进行定位      
|css_selector       | find_elements_by_css_selector     | 通过css选择器进行定位 
```

对象的定位是自动化测试的核心，要想操作一个对象，首先应该识别这个对象。一个对象就像一个人一样，他会有各种的特征（属性），我们可以通过这个属性找到这对象。以百度搜索的输入框为例，有以下属性（F12查看）。

```
<input id="kw" class="s_ipt" name="wd" value="" maxlength="255" autocomplete="off">
```

我们可以这样来定位属性（这里请注意CSS和xpath，本文中不做深入探讨，但是最常用的定位方式）。

```
通过id方式定位：browser.find_element_by_id("kw")
通过name方式定位：browser.find_element_by_name("wd")
通过tag name方式定位：browser.find_element_by_tag_name("input")
通过class name 方式定位：browser.find_element_by_class_name("s_ipt")
通过CSS方式定位：
browser.find_element_by_css_selector("#kw")
browser.find_element_by_css_selector("html > body > form > span > input")
通过xpath方式定位：
browser.find_element_by_xpath("//input[@id='kw']")
browser.find_element_by_xpath("/html/body/form/span/input")
```

复数元素的定位，只要在find_element后面加上s便可，返回的是元素的列表。

```
elements = driver.find_elements_by_xpath('//div/h3/a')
```

## 4.使用：浏览器的操控

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

使用举例。

```
from selenium import webdriver
#1.创建浏览器对象
browser = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
#2.通过浏览器向服务器发送URL请求
browser.get("https://www.baidu.com/")
#3.刷新浏览器
browser.refresh()
#4.设置浏览器的大小
browser.set_window_size(1400,800)
#5.设置链接内容
element=browser.find_element_by_link_text("新闻")
element.click()
```

## 5.使用：鼠标操作

```
| 方法                   | 说明                                                         
| ---------------------- | ------------------------------------------------------------ 
| ActionChains(driver)   | 构造ActionChains对象                                         
| context_click()        | 右击                                                         
| move_to_element(above) | 执行鼠标悬停操作                                             
| double_click()         | 双击                                                         
| drag_and_drop()        | 拖动                                                         
| move_to_element(above) | 执行鼠标悬停操作                                             
| context_click()        | 用于模拟鼠标右键操作， 在调用时需要指定元素定位              
| perform()              | 执行所有 ActionChains 中存储的行为, 可以理解成是对整个操作的提交动作 
```

使用举例。

```
from selenium import webdriver
#1.引入 ActionChains 类
from selenium.webdriver.common.action_chains import ActionChains
driver = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
driver.get("https://www.baidu.com")
#2.定位到要悬停的元素
element= driver.find_element_by_link_text("设置")
#3.对定位到的元素执行鼠标悬停操作
ActionChains(driver).move_to_element(element).perform()
#寻找链接
elem1=driver.find_element_by_link_text("搜索设置")
elem1.click()
```

## 6.使用：键盘操作

```
| 模拟键盘按键               | 说明                
| -------------------------- | ------------------- 
| send_keys(Keys.BACK_SPACE) | 删除键（BackSpace） 
| send_keys(Keys.SPACE)      | 空格键(Space)       
| send_keys(Keys.TAB)        | 制表键(Tab)         
| send_keys(Keys.ESCAPE)     | 回退键（Esc）       
| send_keys(Keys.ENTER)      | 回车键（Enter）     
| send_keys(Keys.CONTROL,'a') | 全选（Ctrl+A） 
| send_keys(Keys.CONTROL,'c') | 复制（Ctrl+C） 
| send_keys(Keys.CONTROL,'x') | 剪切（Ctrl+X） 
| send_keys(Keys.CONTROL,'y') | 粘贴（Ctrl+V） 
| send_keys(Keys.F1…Fn)       | 键盘 F1…Fn     
```

使用举例。

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
browser = webdriver.Edge(executable_path ="D:\GeckoDriver\edgedriver_win64\msedgedriver")
browser.get("https://www.baidu.com")
elem=browser.find_element_by_id("kw")
elem.send_keys('Selenium')
elem.send_keys(Keys.ENTER)
```

## 7.使用：断言

我们可以采用以下属性作为断言参考。

```
| 属性        | 说明                   
| ----------- | ----------------------
| title       | 用于获得当前页面的标题 
| current_url | 用户获得当前页面的URL  
| text        | 获取搜索条目的文本信息 
```

## 8.使用：等待

- 强制等待：sleep，无论浏览器是否加载完成，程序都要等待，等待时间够了才继续执行下面的代码。
- 隐性等待：implicity_wait，`driver.implicitly_wait(30)`隐性等待，最长等30秒。隐性等待设置了一个最长等待时间，如果在规定时间内页面加载完成，则执行下一步，否则一直等到时间截止，然后执行下一步。
- 显性等待：WebDriverWait，需要 until 和 until_not 方法配合使用，`WebDriverWait(driver, 5, 0.5).until(EC.presence_of_element_located((By.ID, "kw")`。其作用是让程序每隔几秒看一眼，如果条件成立了，则执行下一步，否则继续等待，直到超过设置的最长时间，然后抛出 TimeoutException。

## 9.使用：表单切换

```
| 方法                        | 说明                                              
| --------------------------- | -------------------------------------------------- 
| switch_to.frame()           | 将当前定位的主体切换为frame/iframe表单的内嵌页面中
| switch_to.default_content() | 跳回最外层的页面                               
```

## 10.使用：窗口切换

```
| 方法                  | 说明                                                         
| --------------------- | ------------------------------------------------------------ 
| current_window_handle | 获得当前窗口句柄                                             
| window_handles        | 返回所有窗口的句柄到当前会话                                
| switch_to.window()    | 用于切换到相应的窗口
```

## 11.使用：警告框处理

```
| 方法                  | 说明                                               
| --------------------- | -------------------------------------------------- 
| text                  | 返回 alert/confirm/prompt 中的文字信息             
| accept()              | 接受现有警告框                                     
| dismiss()             | 解散现有警告框                                     
| send_keys(keysToSend) | 发送文本至警告框。keysToSend：将文本发送至警告框。 
| switch_to_alert       | 切换到alert
```

## 12.使用：下拉框选择操作

```
from selenium.webdriver.support.select import Select
| 方法                              | 说明                      
| --------------------------------- | ------------------------- 
| select_by_value("选择值")         | select标签的value属性的值 
| select_by_index("索引值")         | 下拉框的索引              
| select_by_visible_testx("文本值") | 下拉框的文本值           
```

## 13.使用：cookie 操作

```
| 方法                              | 说明                                                       
| --------------------------------- | ----------------------------------------------------------
| get_cookies()                     | 获得所有cookie信息                                           
| get_cookie(name)                  | 返回字典的key为“name”的cookie信息                            
| add_cookie(cookie_dict)           | 添加cookie。“cookie_dict”指字典对象，必须有name 和value 值   
| delete_cookie(name,optionsString) | 删除cookie信息。“name”是要删除的cookie的名称，“optionsString”是该cookie的选项
| delete_all_cookies()              | 删除所有cookie信息                                           
```

## 14.使用：窗口截图

```
| 方法                                   | 说明                                
| -------------------------------------- | ------------------------------------ 
| get_screenshot_as_file(self, filename) | 用于截取当前窗口, 并把图片保存到本地 
```

## 15.使用：关闭浏览器

```
| 方法    | 说明         
| ------- | ------------ 
| close() | 关闭单个窗口 
| quit()  | 关闭所有窗口 
```

## 16.使用：获取网站源码

```
| 方法    | 说明         
| ------- | ------------ 
| page_source | 获取网站源码 
```