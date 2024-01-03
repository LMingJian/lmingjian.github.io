---
title: Python 模块：zipfile
date: 2023-11-16
author: LM
---

## 1.简介

zipfile 是 Python 中用于读写 zip 文件的模块，通过构建 zipfile 对象来操作文件。

## 2.单个文件压缩

```python
import zipfile
import os

# 指定要压缩的文件及压缩后的文件名称
zip_file = 'res_data.json'
zip_file_new = zip_file+'.zip'
# 如果文件存在
if not os.path.exists(zip_file):
    print('您要压缩的文件不存在！')
else:
    # step 2: 实例化zipfile对象
    zip = zipfile.ZipFile(zip_file_new, 'w', zipfile.ZIP_DEFLATED)
    # step 3: 写压缩文件
    zip.write(zip_file)

```

## 3.单个目录压缩

```python
import zipfile
import os

month_rank_dir = "test_dir"
zip_file_new = month_rank_dir+'.zip'
if os.path.exists(month_rank_dir):
    print('正在为您压缩...')
    # 压缩后的名字
    zip = zipfile.ZipFile(zip_file_new, 'w', zipfile.ZIP_DEFLATED)
    for dir_path, dir_names, file_names in os.walk(month_rank_dir):
        # 去掉目标跟路径，只对目标文件夹下面的文件及文件夹进行压缩
        fpath = dir_path.replace(month_rank_dir, '')
        for filename in file_names:
            zip.write(os.path.join(dir_path, filename), os.path.join(fpath, filename))
    zip.close()
    print('该目录压缩成功！')
else:
    print('您要压缩的目录不存在...')

```

{{< details "参考文件" >}} 
1：[ Python zip 压缩文件_@CSDN ](https://blog.csdn.net/VinWqx/article/details/108842701)
{{< /details >}}