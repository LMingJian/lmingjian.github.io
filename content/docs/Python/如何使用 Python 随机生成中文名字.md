---
title: 如何使用 Python 随机生成中文名字
date: 2024-12-25T16:10:13+08:00
author: LiangMingJian
---

# 需求

随机生成中文字符名字。

# 单个中文字符的随机生成

## 1.Unicode 编码随机生成

在 Unicode 编码中，汉字的范围是：`[0x4E00, 0x9FFF]`，因此可以从中取值生成中文字符。

需要注意的是，Unicode 编码中收录了 2 万多个汉字，其中包含很多生僻的繁体字。

```python
import random  
  
def unicodeString():  
    val = random.randint(0x4E00, 0x9FFF)  
    return chr(val)
```

`chr(int)` 方法能将符合格式的十六进制数值按 Unicode 编码转换为对应的字符。

> `ord(str)` 是 `chr(int)` 的逆操作，`ord(str)` 将目标字符转换为对应的 Unicode 编码。  
>   
> 另外，`ord(str)` 转换结果是以十进制展示的，如果需要查看对应的十六进制代码，需要使用 `hex(int)` 转换。比如：`ord('一') == 19968, hex(19968) == 0x4e00` 。

## 2.GB2312 编码随机生成

在 GB2312 编码中，汉字的范围是：`[0xB0A1, 0xF7FE]`，同样可以从中取值生成中文字符。

需要注意的是，GB2312 编码收录 6 千多个汉字，比 Unicode 少，但对应的生僻字也少。

此外，GB2312 编码是双字节编码，即 `0xB0` 和 `0xA1` 两两结合才是一个字符，而不是像 Unicode 编码，以 `0xB0A1` 作为一个字符。因此头字节范围有 `[0xB0, 0xF7]`，尾字节范围有 `[0xA1, 0xFE]`。

```python
import random

def gb2312String():  
    start = random.randint(0xB0, 0xF7)  
    end = random.randint(0xA1, 0xFE)  
    val = f'{start:x}{end:x}'  
    string = bytes.fromhex(val).decode('gb2312')  
    return string
```

`f'{start:x}{end:x}'` 使用 `:x` 将输入的数据以小写十六位字符拼接。如果需要大写，则使用 `:X`。如果需要补齐 4 位，则使用 `:04x`。（注意，不这样处理 Python 会将传入的数值直接变成十进制）

`bytes.fromhex(val)` 用于将一个十六进制格式的字符串变为数值，进行类型转换。

# 3.从列表随机生成

除了上面从编码中获取的方法外，还可以通过字符列表，收录常用的汉字，然后随机取值生成。

```python
import random

def listString():  
    string_list = ['秀', '娟', '英', '华', '慧', '巧', '美', '娜']  
    return random.choice(string_list)
```

`random.choice()` 用于随机从序列中获取一个值。

# 拼接为两个或三个字符的名字

下面以从列表随机生成为例，提供一个常用汉字列表与一个百家姓列表，从这两个列表中随机取值生成一个标准的中文名字。

```python
def create_name():  
    first_name_list = [  
        "赵", "钱", "孙", "李", "周", "吴", "郑", "王", "冯", "陈",  
        "褚", "卫", "蒋", "沈", "韩", "杨", "朱", "秦", "尤", "许",  
        "何", "吕", "施", "张", "孔", "曹", "严", "华", "金", "魏",  
        "陶", "姜", "戚", "谢", "邹", "喻", "柏", "水", "窦", "章",  
        "云", "苏", "潘", "葛", "奚", "范", "彭", "郎", "鲁", "韦",  
        "昌", "马", "苗", "凤", "花", "方", "俞", "任", "袁", "柳",  
        "酆", "鲍", "史", "唐", "费", "廉", "岑", "薛", "雷", "贺",  
        "倪", "汤", "滕", "殷", "罗", "毕", "郝", "邬", "安", "常",  
        "乐", "于", "时", "傅", "皮", "卞", "齐", "康", "伍", "余",  
        "元", "卜", "顾", "孟", "平", "黄", "和", "穆", "萧", "尹"  
    ]  
  
    second_name_list = ['秀', '娟', '英', '华', '慧', '巧', '美', '娜']  
  
    first_name = random.choice(first_name_list)  
  
    second_name_count = random.randint(1, 2)  
    second_name = ''.join(random.choices(second_name_list, k=second_name_count))  
  
    return first_name + second_name
```

`random.choices()` 用于随机从序列中抽取元素并返回为一个列表，可以通过参数 k 控制随机抽取元素的数量，支持重复抽取。

`''.join` 用于将列表中的元素按特定字符 `''` 进行拼接返回。
