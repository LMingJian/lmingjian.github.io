---
title: Python 嵌套循环的跳出
date: 2022-09-03
author: LM
---

## 1.需求

从嵌套循环中跳出，结束循环。

## 2.实现：使用"旗帜"变量

```python
# 添加"旗帜"变量
break_out_flag = False
for i in range(5):
    for j in range(5):
        if j == 2 and i == 0:
            break_out_flag = True
            break
    if break_out_flag:
        break
```


在上面的代码中，变量 break_out_flag 作为一个旗帜告诉程序该跳出这个外循环。

## 3.实现：主动引发异常

```python
# 主动引发异常
try:
    for i in range(5):
        for j in range(5):
            if j == 2 and i == 0:
                raise StopIteration
except StopIteration:
    pass
```

在上面的代码中，我们把异常当做关键词 break 使用，通过主动抛出异常来跳出所有循环。

## 4.实现： 使用相同的条件语句

```python
# 使用相同的条件语句
for i in range(5):
    for j in range(5):
        if j == 2 and i == 0:
            break
    if j == 2 and i == 0:
        break
```

由于一个条件语句可以中断一次循环，那么使用相同的条件语句同样也可以再一次中断一个循环。

## 5.实现：使用 For-Else 语句

```python
# 使用 For-Else 语句
for i in range(5):
    for j in range(5):
        if j == 2 and i == 0:
            break
    else:  # 仅在内循环不中断时执行
        continue
    break
```

在 For-Else 语句中， else 下面的语句只有当内循环执行完成且没有任何中断时才会执行。当内循环被 break 时，else 下的语句将会被跳过，For-Else 的意思是执行循环，然后再干什么。

## 6.实现：将嵌套循环放在一个函数里

```python
# 将嵌套循环放在一个函数里
def check_sth():
    for i in range(5):
        for j in range(5):
            if j == 2 and i == 0:
                return
check_sth() # 执行这个函数
```


将嵌套循环放在一个函数中，当需要跳出嵌套循环时，便可以使用 return 这个关键词替代 break 。

{{< details "参考文件" >}} 
1：[ Python 跳出嵌套循环的5种方法_@GLUT.Y ](https://blog.csdn.net/lyf6667/article/details/123488637)
{{< /details >}}