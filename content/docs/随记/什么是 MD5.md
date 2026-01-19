---
title: 什么是 MD5
date: 2024-12-25T10:33:40+08:00
author: LiangMingJian
---

# MD5 的概述

MD5，Message-Digest Algorithm 5，信息摘要算法 5，是一种被广泛使用的密码散列函数算法。MD5 将给定的原数据进行散列处理，然后生成一个 128 位（16 字节）的散列值（Hash），用来确保信息传输过程中的安全和完整。

# MD5 的特点

1. **压缩性**：任意长度的数据，经过 MD5 算法处理后，都会生成一个固定长度的 128 位散列值。 
2. **容易计算**：从原数据计算出 MD5 值非常容易。
3. **抗修改性**：对原数据进行任何改动，哪怕只修改 1 个字节，所得到的 MD5 值都会有很大区别。
4. **弱抗碰撞**：即使拥有原数据和其对应的 MD5 值，但想找到一个具有相同 MD5 值的数据是几乎不可能的。
5. **强抗碰撞**：想找到两个不同的数据，使它们具有相同的 MD5 值，也是几乎不可能的。

注意，**MD5 无法完全防止碰撞**，上述的弱抗碰撞和强抗碰撞所指的几乎不可能描述的是只是概览低，但并不是零概率，即，想找到两个不同的数据，使它们具有相同的 MD5 值的概率很小，但并不是完全不可能。

# MD5 的应用

1. **密码存储**：在数据库中存储密码时，存储密码的 MD5 值而不是明文密码，来保证密码的安全性。
2. **数字签名**：对文件进行 MD5 计算，可以生成文件的指纹，可以在文件传输后用于验证文件的完整性，避免文件被修改或下载不完整。
3. **防止数据篡改**：在通信过程中，对发送的信息进行 MD5 计算，可以通过比较发送前后的 MD5 值，避免数据被篡改。

# MD5 的 Python 实现

在 Python 中，可以通过 `hashlib` 库来实现 MD5 值的计算。

```python
import hashlib  
  
  
def md5_string(data: str):  
    md5_hash = hashlib.md5()  
    md5_hash.update(data.encode('utf-8'))  
    return md5_hash.hexdigest()  
  
  
def md5_file(file_path: str):  
    md5_hash = hashlib.md5()  
    with open(file_path, 'rb') as f:  
        for chunk in iter(lambda: f.read(4096), b''):  
            md5_hash.update(chunk)  
    return md5_hash.hexdigest()  
  
  
print(f"MD5: {md5_string('Hello Python')}")  
# MD5: a709c173220d6185d12248faa9f40ac8
print(f"MD5: {md5_file('1.txt')}")
# MD5: e10adc3949ba59abbe56e057f20f883e
```

# MD5 的 Windows 使用

在 Windows 系统中，可以用 PowerShell 的 `Get-FileHash` 命令来计算文件的 MD5 值。

```shell
Get-FileHash 1.txt -Algorithm MD5
# Algorithm       Hash                                                                   Path                                                   
# ---------       ----                                                                   ---- 
# MD5             E10ADC3949BA59ABBE56E057F20F883E                                       F:\MyPython\PythonProject313\1.txt
```

# MD5 的 Linux 使用

在 Linux 系统中，可以使用 `md5sum` 命令来计算文件的 MD5 值。

```shell
md5sum README.txt
# 7ae60f934ac65b0386316aed67150b45  README.txt
```
