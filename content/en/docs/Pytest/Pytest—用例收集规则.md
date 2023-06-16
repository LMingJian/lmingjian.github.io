---
title: Pytest 测试用例的收集规则
date: 2022-01-07
author: LM
---

## 1.默认的执行顺序

pytest 默认执行顺序是按照 case 在代码中的顺序位置先后执行的。case 的收集默认从当前运行目录开始查找文件，该查找为递归查找，子目录中的文件也会被查找。pytest 能且仅能查找符合命名规则的 py 文件，默认规则是以`test _`开头或者以`test`结尾的 py 文件。

## 2.指定查找

使用同目录下的`pytest.ini`或`conftest.py`来改变搜索顺序。当在配置文件中指定目录后，pytest 就会从该目录中开始查找测试用例文件。`testpaths = ./scripts`  

{{< details "参考文件" >}} 
1：[ 用例收集规则 @jaxon-chen ](https://www.cnblogs.com/jaxon-chen/p/13204625.html)
{{< /details >}}