---
title: Python Package：zipfile
date: 2024-12-25T16:12:58+08:00
author: LiangMingJian
---

# 概述

> 标准库

zipfile 是 Python 中用于读写 zip 文件的一个模块，其通过构建 zipfile 实例对象完成对文件的操作，支持创建，读取，写入，添加等功能。

需要注意，当前该模块不能处理分卷压缩的 ZIP 文件，同时也不建议处理加密后的 ZIP 文件（处理很慢），当然，也不能创建一个加密的 ZIP 文件。

# 压缩文件

通过构建 ZipFile 对象，并通过 `write()` 方法添加文件进行压缩。

`class zipfile.ZipFile()` 支持参数包括：

- `file`：必填，传入一个文件路径或文件对象。
- `mode`：对象的操作模式，与文件一样，支持读 `r`，写 `w`，追加 `a`，仅新建 `x`。
- `compression`：使用的 ZIP 压缩方法，支持的压缩方法包括：`ZIP_STORED`（仅归档不不压缩），`ZIP_DEFLATED`（使用 zlib 模块压缩），`ZIP_BZIP2` （使用 bz2 模块压缩）或 `ZIP_LZMA`（使用 lzma 模块压缩）（如果确实模块，则压缩失败）。
- `allowZip64`：当 zipfile 大于 4 GiB 时，将创建使用 ZIP64 扩展的 ZIP 文件。
- `compresslevel`：压缩等级，0-9。
- `strict_timestamps`：禁止压缩早于 1980-01-01 的文件（ ZIP 文件的时间戳存储采用 MS-DOS 格式，仅支持 1980 年（MS-DOS 的发布时间）之后的时间）。
- `metadata_encoding`：元数据编码格式，如：UTF-8。

```python
import zipfile  

# 新建一个压缩文件
with zipfile.ZipFile('example.zip', 'w') as myzip:  
    myzip.write('requirements.txt')

# 仅新建一个压缩文件（如果文件已存在则会报错）
with zipfile.ZipFile('example.zip', 'x') as myzip:  
    myzip.write('requirements.txt')
    # 报错：FileExistsError: [Errno 17] File exists: 'example.zip'

# 添加文件到一个已有压缩文件
with zipfile.ZipFile('example.zip', 'a') as myzip:  
    myzip.write('my.xls')
```

# 压缩目录

当需要压缩一个文件夹时，开发者要结合 `os.walk()` 递归目标目录下所有的内容，然后将这些内容全部写入到压缩文件中。

需要注意，在添加文件时要保留目录下所有文件的路径结构，不然压缩包里面的文件夹会错乱。

```python
import os  
import zipfile  
  
target_path = 'example/'  
with zipfile.ZipFile('example.zip', 'w') as myzip:  
    for folder_path, folder_names, file_names in os.walk(target_path):  
        for file in file_names:  
            file_path = os.path.join(folder_path, file)  
            arcname = os.path.relpath(file_path, target_path)  
            myzip.write(file_path, arcname)  
            # myzip.write(文件路径，归档名称)，归档名称可选
```

# 解压 ZIP 文件

通过 `extract()` 可以单独从压缩包中提取一个文件，通过 `extractall()` 可以解压所有文件。

```python
import zipfile  

# 解压相对压缩包路径为 exp/null.py 的文件到当前文件夹
with zipfile.ZipFile('example.zip', 'r') as myzip:  
    myzip.extract('exp/null.py', './')

# 解压压缩包内 base.py, tp.js 这两个文件到当前文件夹
with zipfile.ZipFile('example.zip', 'r') as myzip:  
    myzip.extractall('./', ['base.py', 'tp.js'])

# 解压压缩包内所有文件到当前文件夹
with zipfile.ZipFile('example.zip', 'r') as myzip:  
    myzip.extractall('./')
```

# 查看 ZIP 文件

```python
import zipfile  
  
with zipfile.ZipFile('example.zip', 'r') as myzip: 
    # 将压缩包的目录信息打印到控制台
    myzip.printdir()
    # 返回压缩包内所有文件名称列表
    print(myzip.namelist())  
    # 返回压缩包内所有文件的 ZipInfo 对象
    print(myzip.infolist())
    # 返回压缩包内某个文件的 ZipInfo 对象并查看相关信息
    iinfo = myzip.getinfo('base.py')  
    print(info.is_dir())       # 是不是文件夹
    print(info.filename)       # 文件名
    print(info.date_time)      # 创建时间
    print(info.file_size)      # 未压缩前的大小
    print(info.compress_size)  # 压缩后的大小
```

————————————

[ zipfile --- 操作 ZIP 归档文件 ](https://docs.python.org/zh-cn/3/library/zipfile.html)
