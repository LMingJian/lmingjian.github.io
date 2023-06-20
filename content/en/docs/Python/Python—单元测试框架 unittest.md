---
title: Python 的单元测试框架 unittest
date: 2020-11-30
author: LM
---

## 1.单元测试框架

Unittest 是 Python 内部自带的一个单元测试的模块。它具备完整的测试结构，支持自动化测试的执行，对测试用例集进行组织，并且提供了丰富的断言方法，最后生成测试报告。

## 2.测试用例 Test Case

一个 TestCase 的实例就是一个测试用例。一个完整的测试流程，包括测试前准备环境的搭建，执行测试代码，以及测试后环境的还原。

```python
class testcase(unittest.TestCase):
    @classmethod
    def setUpClass(self):
        # 所有的测试方法运行前运行，整个测试过程中只执行一次。
        pass
    def setUp(self):
        # 每个测试方法运行前运行，一条用例执行一次，若N次用例就执行N次。
        pass
    def tearDown(self):
        # 每个测试方法运行结束后运行,一条用例执行一次，若N次用例就执行N次。
    @classmethod
    def tearDownClass(self):
        # 所有的测试方法运行结束后运行，整个测试过程中只执行一次。
        pass
    def test_01(self):
        # 测试用例
        pass
```

## 3.测试集 Test Suite

多个测试用例集合在一起，就是 TestSuite。

```python
if __name__ == "__main__":
    # 构造 TestSuite
    suite = unittest.TestSuite()
    suite.addTest(testcase("test_01"))
    suite.addTest(testcase("test_02"))
    # 执行测试
    runner = unittest.TextTestRunner()
    runner.run(suite)
```

## 4.测试加载器 Test Loader

加载 TestCase 到 TestSuite 中。

```python
if __name__ == "__main__":
    # 一个一个文件加载
    suite1 = unittest.TestLoader().loadTestsFromTestCase(TestCase1)
    suite2 = unittest.TestLoader().loadTestsFromTestCase(TestCase2)
    suite = unittest.TestSuite([suite1, suite2])
    # 自动匹配并加载
    test_dir = "./test_case"
    discover = unittest.defaultTestLoader.discover(test_dir, pattern="test*.py")
    runner=unittest.TextTestRunner()
    runner.run(discover)
```

## 5.断言

```python
assertEqual(a, b) #  a == b
assertNotEqual(a, b)  # a != b
assertTrue(x) #  bool(x) is True
assertFalse(x) #  bool(x) is False
assertIs(a, b)  # a is b
assertIsNot(a, b)  # a is not b
assertIsNone(x) #  x is None
assertIsNotNone(x) #  x is not None
assertIn(a, b)  # a in b
assertNotIn(a, b) #  a not in b
assertIsInstance(a, b) #  isinstance(a, b)
assertNotIsInstance(a, b) #  not isinstance(a, b)
```

## 6.unittest 网易邮箱登录案例

```python
import time
import unittest from selenium 
import webdriver from selenium.webdriver.support 
import expected_conditions as EC

class mailLogin(unittest.TestCase):
    def setUp(self):
        url = 'https://mail.yeah.net/'
        self.browser = webdriver.Firefox()
        self.browser.get(url)
        time.sleep(5)
    
    def test_login_01(self):
        '''
        用户名、密码为空
        '''
        self.browser.switch_to.frame("x-URS-iframe")
        self.browser.find_element_by_name('email').send_keys('')
        self.browser.find_element_by_name('password').send_keys('')
        self.browser.find_element_by_id('dologin').click()
        self.browser.switch_to.default_content()
        time.sleep(3)
        name = self.browser.find_element_by_id('spnUid')        
        if name == 'sanzang520@yeah.net':
            print('登录成功')        
        else:
            print('登陆失败')    
            
    def test_login_02(self):
        '''
        用户名正确、密码为错误
        '''
        self.browser.switch_to.frame("x-URS-iframe")
        self.browser.find_element_by_name('email').send_keys('sanzang520')
        self.browser.find_element_by_name('password').send_keys('xxx')
        self.browser.find_element_by_id('dologin').click()
        self.browser.switch_to.default_content()
        time.sleep(3)
        name = self.browser.find_element_by_id('spnUid')        
        if name == 'sanzang520@yeah.net':
            print('登录成功')        
        else:
            print('登陆失败')    
    
    def test_login_03(self):
        '''
        用户名、密码正确
        '''
        self.browser.switch_to.frame("x-URS-iframe")
        self.browser.find_element_by_name('email').send_keys('sanzang520')
        self.browser.find_element_by_name('password').send_keys('xxx')
        self.browser.find_element_by_id('dologin').click()
        self.browser.switch_to.default_content()
        time.sleep(3)
        name = self.browser.find_element_by_id('spnUid')        
        if name == 'sanzang520@yeah.net':
            print('登录成功')        
        else:
            print('登陆失败')    
            
    def tearDown(self):
        self.browser.quit()
        
if __name__ == "__main__":
    unittest.main()
```