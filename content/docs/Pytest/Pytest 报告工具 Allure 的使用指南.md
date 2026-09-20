---
title: Pytest 报告工具 Allure 的使用指南
date: 2026-05-15T11:00:05+08:00
author: LiangMingJian
---

# 概述

Allure 是一个轻量级、灵活的、支持多语言、支持多平台的测试报告生成工具，其可以生成美观，详细的 HTML 测试报告。

Allure 的核心优势在于：
- **界面美观**：可以提供美观的 HTML 报告。
- **信息丰富**：可以记录包括测试步骤、测试附件、日志、截图在内的多种数据。
- **多语言支持**：支持 Java、Python、JavaScript、PHP、Ruby 等多种编程语言。
- **框架集成友好**：与主流的测试框架（Pytest、JUnit、TestNG 等）集成良好

在本文中，主要以 Pytest 为测试框架介绍 Allure 的使用。

# 安装

## 安装 Allure 命令行工具

与其他 Pytest 的测试报告工具（可以通过 pip 安装）不同，Allure 是一个独立的应用工具，需要单独下载安装。

通过访问 Allure 项目仓库 [allure2@github](https://github.com/allure-framework/allure2/releases) 下载安装。

> 下载并解压后，务必将项目的 `bin` 目录添加到系统环境变量中。

在命令行终端执行以下命令验证安装。

```bash
allure --version
```

## 安装 Pytest 插件

在安装 Allure 后，我们还需要在 Python 项目中，安装 `allure-pytest` 插件来让 Pytest 生成 Allure 格式的测试报告。

```bash
pip install allure-pytest
```

# 基本使用

## 报告生成

Pytest 可以通过参数 `--alluredir` 来让程序在执行完成测试后生成 Allure 格式的测试报告。

> 需要注意的是，这个报告如果需要查看，则还需要 Allure 命令行工具的帮助。

```bash
# --alluredir 指定报告目录，--clean-alluredir 生成前先清除该目录
pytest --alluredir=./allure-results --clean-alluredir
```

## 报告查看

在本地调试时，Allure 报告可以通过执行 `allure serve` 查看。

```bash
allure serve ./allure-results -h 0.0.0.0 -p 8080
```

## 报告打包

除了在本地查看，有时候我们还需要将报告给其他人看。

此时，我们可以通过 `allure generate` 命令生成静态 HTML 文件，通过传输文件或部署文件到 Web 服务器来给其他人观看。

```bash
# 打包报告到指定目录，--clean 打包前先清除该目录
allure generate ./allure-results -o ./allure-report --clean
```

# Pytest 集成使用

在 Pytest 中，Allure 为其提供了大量的装饰器，用于提高测试报告信息的详细程度。

## 用例的基础信息

比如，可以使用装饰器 `@allure.feature()` 与 `@allure.story()` 来标记功能模块与用户故事。

```python
import allure

@allure.feature('用户管理')
@allure.story('用户登录')
def test_user_login():
    pass

@allure.feature('用户管理')
@allure.story('用户注册')
def test_user_register():
    pass
```

此外，也可以使用装饰器 `@allure.title()`，`@allure.description()`，`@allure.severity()` 来标记一个测试用例的标题，描述，以及严重级别。

```python
@allure.title('用户成功登录')
@allure.description('输入正确的用户名和密码，是否成功登录')
@allure.severity(allure.severity_level.CRITICAL)
def test_login_success():
    pass
```

严重级别 `@allure.severity()` 支持以下选项：

- `allure.severity_level.BLOCKER`：阻塞
- `allure.severity_level.CRITICAL`：严重
- `allure.severity_level.NORMAL`：一般
- `allure.severity_level.MINOR`：轻微
- `allure.severity_level.TRIVIAL`：建议

特别的，有时候 Pytest 会使用参数化的形式创建测试用例，这个时候，测试用例的信息可以通过 `allure.dynamic.xxxx()` 的方式动态创建。

```python
import allure
import pytest

@pytest.mark.parametrize('username, password', [
    ('user1', 'password1'),
    ('user2', 'password2'),
])
def test_login(username, password):
    """用户登录"""
    allure.dynamic.feature('用户管理')
    allure.dynamic.story(f'用户登录')
    allure.dynamic.title(f'测试登录用户：{username}')
    allure.dynamic.description(f'验证用户 {username} 是否成功登录')
    allure.dynamic.severity(allure.severity_level.CRITICAL)
```

## 测试步骤

在 Allure 中，通过装饰器 `@allure.step()` 可以将代码变成测试步骤记录下来。

```python
import allure

@allure.feature('购物模块')
@allure.story('下单')
def test_place_order():
    # 步骤1：登录
    with allure.step('用户登录'):
        username = "user"
        password = "password"
        login(username, password)
    
    # 步骤2：添加商品
    with allure.step('添加商品到购物车'):
        product_id = "P1"
        quantity = 100
        add_to_cart(product_id, quantity)
    
    # 步骤3：提交订单
    with allure.step('提交订单'):
        order_id = "O1"
        submit_order(order_id)
```

## 附件管理

最后，Allure 还支持在报告中添加各种类型的附件，包括但不限于文本，图片，链接。

**文本附件**

```python
import allure

def test_text_attachment():
    # 添加文本内容
    allure.attach(
        body='这是测试日志内容',
        name='测试日志',
        attachment_type=allure.attachment_type.TEXT
    )
    
    # 添加 HTML 内容
    allure.attach(
        body='<h1>HTML报告</h1><p>段落内容</p>',
        name='HTML附件',
        attachment_type=allure.attachment_type.HTML
    )
```

**图片附件**

```python
import allure

def test_screenshot():
    # 从文件导入图片，图片支持 PNG，JPG，SVG，GIF
    allure.attach.file(
        source='./screenshots/error.png',
        name='错误截图',
        attachment_type=allure.attachment_type.PNG
    )
```

**其他附件**

```python
import allure

def test_various_attachments():
    # JSON 数据
    allure.attach(
        body='{"name": "test", "value": 123}',
        name='JSON数据',
        attachment_type=allure.attachment_type.JSON
    )
    
    # CSV 数据
    allure.attach(
        body='id,name,value\n1,test,100',
        name='CSV数据',
        attachment_type=allure.attachment_type.CSV
    )
    
    # XML 数据
    allure.attach(
        body='<?xml version="1.0"?><root><item>test</item></root>',
        name='XML数据',
        attachment_type=allure.attachment_type.XML
    )
```

特别的，链接附件需要通过装饰器来添加。

```python
@allure.link('https://example.com/docs', name='相关文档')
@allure.issue('https://example.com/BUG-1', name='关联Bug')
@allure.testcase('https://example.com/CASE-1', name='测试用例')
def test_example():
    pass
```

————————————

> 官方文档：[Allure Framework](https://docs.qameta.io/allure-report/)
