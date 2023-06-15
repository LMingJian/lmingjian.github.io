---
title: Linux 中 Chrome 浏览器的安装
date: 2021-05-15
author: LM
---

## 1.Chrome 安装

不同于 Windows，在最小化安装，无用户界面的 Linux 上安装 Chrome 浏览器会有点复杂，需要安装多样的依赖。

```python
# 下载-------------------------------------------
wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
# 安装依赖-------------------------------------------------------------
yum install libX11 libXcursor libXdamage libXext libXcomposite libXi libXrandr gtk3 libappindicator-gtk3 xdg-utils libXScrnSaver liberation-fonts
# 执行安装--------------------------------------------------------------------
rpm -ivh google-chrome-stable_current_x86_64.rpm
# 检验版本
google-chrome --version
```

## 2.chromedriver 配置使用

在自动化测试中，使用无头浏览器进行测试。

```python
# 前往 https://npm.taobao.org/mirrors/chromedriver/ 下载对应的 A-WebDriver
# 建议将 A-WebDriver 上传到路径 /usr/local/bin 以添加到环境中，这样在编写脚本时不需要指定路径
# 参数配置----------------------------------------------------------
class webs(unittest.TestCase):
    def setUp(self):
        options = ChromeOptions()
        options.add_argument('--no-sandbox') # 不在沙盒运行，以Root权限运行
        options.add_argument('--headless') # 必须添加这两个参数，否则会出现权限不够的问题
        self.browser = Chrome(options=options) # WebDriver已添加到环境中
```

