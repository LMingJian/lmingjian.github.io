---
title: 如何在 Python 中跳出多层嵌套循环
date: 2024-12-25T16:10:38+08:00
author: LiangMingJian
---

# 需求

这里有一个多层的嵌套循环：

```python
for x in range(5):
    for y in range(6):
        for z in range(8):
            pass
```

此时，用户要求在最里面一层循环达到一定条件后，直接结束所有循环，不执行完所有循环。

# 方法一：设置 Flag

在循环外面添加 Flag 变量，然后在循环时变更变量，通过判定 Flag 的值来跳出循环。

这种做法的缺点是，每层循环都需要判断，都需要执行 break 跳出。

```python
flag = False
for x in range(5):
    for y in range(6):
        for z in range(8):
            if x == 1 and y == 2 and z == 3:
                flag = True
                break
        if flag:
            break
    if flag:
        break
```

# 方法二：通过主动抛出异常跳出循环

在循环外层嵌套一个 `try ... except ...` 语句，然后循环内部通过 `raise` 主动抛出异常来结束所有循环。

```python
try:
    for x in range(5):
        for y in range(6):
            for z in range(8):
                if x == 1 and y == 2 and z == 3:
                    raise StopIteration
except StopIteration:
    pass
```

# 方法三：使用 for ... else ...

在 Python 中，`for ... else ...` 语句的 `else` 只有在循环体正常执行完成且没有中断的时候才会执行，当循环体被 `break`，则 `else` 后面的代码会直接跳过，继续执行后续代码。

正如下面代码，最里层循环如果正常执行，则会触发 `else` 后面的语句，然后执行 `continue` 直接进行下一次循环，当被 `break` 时，则 `else` 不会被执行，`continue` 不会被执行，外层的 `break` 将被执行。

```python
for x in range(5):
    for y in range(6):
        for z in range(8):
            if x == 1 and y == 2 and z == 3:
                break
        else:
            continue
        break
    else:
        continue
    break
```

# 方法四：封装为函数，使用 return 跳出循环

将多层嵌套循环封装在一个函数内，然后通过 `return` 直接结束函数运行，从而跳出嵌套循环。

```python
 def loop():
    for x in range(5):
        for y in range(6):
            for z in range(8):
                if x == 1 and y == 2 and z == 3:
                    return 0
    return 1

loop()
```
