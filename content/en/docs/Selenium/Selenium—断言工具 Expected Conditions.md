---
title: Selenium 的断言工具 Expected Conditions
date: 2021-07-30
author: LM
---

## 1.简介

Expected Conditions 是 Selenium 提供的断言工具，通常使用场景有以下的 2 种。

- 直接在断言中使用。
- 与 WebDriverWait 配合使用，动态等待页面上元素出现或者消失。

## 2.使用

```python
dr = webdriver.Chrome()
url = 'https://www.baidu.com'
search_text_field_id = 'kw'
dr.get(url)

class ECExample(unittest.TestCase):

  def test_title_is(self):
    ''' 判断title是否符合预期 '''
    title_is_baidu = EC.title_is(u'百度一下，你就知道')
    self.assertTrue(title_is_baidu(dr))

  def test_titile_contains(self):
    ''' 判断title是否包含预期字符 '''
    title_should_contains_baidu = EC.title_contains(u'百度')
    self.assertTrue(title_should_contains_baidu(dr))

  def test_presence_of_element_located(self):
    ''' 判断element是否出现在dom树 '''
    locator = (By.ID, search_text_field_id)
    search_text_field_should_present = EC.visibility_of_element_located(locator)

    ''' 动态等待10s，如果10s内element加载完成则继续执行下面的代码，否则抛出异常 '''
    WebDriverWait(dr, 10).until(EC.presence_of_element_located(locator))
    WebDriverWait(dr, 10).until(EC.visibility_of_element_located(locator))

    self.assertTrue(search_text_field_should_present(dr))

  def test_visibility_of(self):
    search_text_field = dr.find_element_by_id(search_text_field_id)
    search_text_field_should_visible = EC.visibility_of(search_text_field)
    self.assertTrue(search_text_field_should_visible('yes'))

  def test_text_to_be_present_in_element(self):
    text_should_present = EC.text_to_be_present_in_element((By.NAME, 'tj_trhao123'), 'hao123')
    self.assertTrue(text_should_present(dr))
    
  @classmethod
  def tearDownClass(kls):
    print 'after all test'
    dr.quit()
    print 'quit dr'

if __name__ == '__main__':
  unittest.main()
```

## 3.方法

| 序号 | 函数名                                 | 描述                                                         | 返回值     |
| ---- | -------------------------------------- | ------------------------------------------------------------ | ---------- |
| 1    | title_is                               | 判断当前页面的 title 是否精确等于预期                        | 布尔       |
| 2    | title_contains                         | 判断当前页面的 title 是否包含预期字符串                      | 布尔       |
| 3    | presence_of_element_located            | 判断元素是否被加到了 DOM 树里，并不代表该元素一定可见        | WebElement |
| 4    | visibility_of_element_located          | 判断元素是否被加到了 DOM 树里，并且可见，并且元素的宽和高都不等于 0 | WebElement |
| 5    | visibility_of                          | 判断元素是否可见，如果可见则返回这个元素                     | WebElement |
| 6    | presence_of_all_elements_located       | 判断是否至少有 1 个元素存在于 DOM 树中                       | 列表       |
| 7    | visibility_of_any_elements_located     | 判断是否至少有 1 个元素在页面上可见                          | 列表       |
| 8    | text_to_be_present_in_element          | 判断元素的 text 是否包含了预期的字符串                       | 布尔       |
| 9    | text_to_be_present_in_element_value    | 判断元素的 value 是否包含了预期的字符串                      | 布尔       |
| 10   | frame_to_be_available_and_switch_to_it | 判断该 frame 是否可以切换进去                                | 布尔       |
| 11   | invisibility_of_element_located        | 判断某个元素中是否不存在于 DOM 树或不可见                    | 布尔       |
| 12   | element_to_be_clickable                | 判断某个元素中是否可见并且是 enable 可操作的                 | 布尔       |
| 13   | staleness_of                           | 判断某个元素是否从 DOM 树中移除                              | 布尔       |
| 14   | element_to_be_selected                 | 判断某个元素是否被选中了，一般用在下拉列表                   | 布尔       |
| 15   | element_selection_state_to_be          | 判断某个元素的选中状态是否符合预期                           | 布尔       |
| 16   | element_located_selection_state_to_be  | 判断某个元素的选中状态是否符合预期                           | 布尔       |
| 17   | alert_is_present                       | 判断页面上是否存在 alert                                     | 布尔       |



{{< details "参考文件" >}} 
1：[ selenium expected conditions 使用实例  @乙醇 ](https://www.cnblogs.com/nbkhic/p/4885041.html)
{{< /details >}}