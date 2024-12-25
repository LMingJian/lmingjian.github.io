---
title: Allure Report 的 Pytest 使用
date: 2024-12-25T11:00:00+08:00
author: LiangMingJian
---

# 生成报告

```bash
pytest --alluredir=/tmp/my_allure_results  # 执行测试并生成报告
```

# 附件

```python
# 使用 allure.attach() 来插入一段自己写的 HTML 
# 或 allure.attach.file() 来导入一个已存在的 HTML 文件
allure.attach(body, name, attachment_type, extension)
allure.attach.file(source, name, attachment_type, extension)
"""
body：要显示的内容（附件）
name：附件名字
attachment_type：附件类型，是 allure.attachment_type 里面的其中一种
extension：附件的扩展名（比较少用）
source：文件路径，相当于传一个文件
"""
```

# 语法糖

```python
@allure.feature('功能名称')
@allure.issue("测试网站") 
@allure.link("链接")
@allure.testcase("测试ID")
@allure.description("说明")
@allure.story('子功能名称') 
@allure.title('标题')
@allure.step('步骤')
@allure.severity('级别')
```

# 用例的描述

```python
# 注释即为描述
"""
描述内容
"""
```
