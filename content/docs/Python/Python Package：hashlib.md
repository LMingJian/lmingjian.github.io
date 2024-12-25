---
title: Python Package：hashlib
date: 2024-12-25T16:11:29+08:00
author: LiangMingJian
---

# 概述

hashlib 是 Python 中支持各类 hash 算法的一个模块，可以使用该模块对数据进行加密，解密。

# 使用

```python
import hashlib

if __name__ == "__main__":
    s = hashlib.sha256()    # Get the hash algorithm.
    s.update("jiangwei")    # Hash the data.
    b = s.hexdigest()       # Get he hash value.
    print b

"""
>>>36d6824c3942c1ec34d1b5cc2f5cbe3d2c4110223bce905ddafe0592723411f0
# 它的长度是64，每个字符4位，所以总计是256位。
>>>s = hashlib.sha512()    # Get the hash algorithm.
>>>3ee15fe269635e2c05d7e965d97d82b95fefcdf7f937cb14e117d235a440b9e173d90f3c669a5dce21d6b8a20ff2376172d171d0c9d9b1f2670d39aefaab7a10
# 可以得到512位的摘要
"""
```

# EX.加密算法 SHA256

SHA256 的全称是 Secure Hash Algorithm 256，一种常用的加密算法，是 SHA-2 族算法中的一个，其它的还是 SHA222、SHA512 等。

- Secure 的意思是指算法的输入输出一一对应，且是不可逆的（即只有编码而没有解码）
- Hash Algorithm 指的是散列算法，散列算法指的是将一个任意长度的输入数据转换成固定长度的输出
- 256 是输出结果的位数，这个输出结果又被称为 Hash 值或者摘要
