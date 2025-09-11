---
title: Python Package：hashlib
date: 2024-12-25T16:11:29+08:00
author: LiangMingJian
---

# 概述

> 标准库

hashlib 是 Python 中的一个内置模块，其提供了多种不同哈希算法的实现。开发者可以通过该模块对敏感数据进行加密操作。

hashlib 支持的算法包括了 SHA224，SHA256，SHA384，SHA512，SHA-3 等在内的 FIPS 安全哈希算法，同时还包括旧式算法 SHA1 和 MD5。

# 对数据哈希

hashlib 提供包括多种哈希算法的构造器，通过往构造器里传入数据，可以按对应的哈希算法进行加密。支持的构造器算法包括：`sha1(), sha224(), sha256(), sha384(), sha512(), sha3_224(), sha3_256(), sha3_384(), sha3_512(), shake_128(), shake_256(), blake2b(), blake2s(), md5()`

以 sha256 为例，将数据 Python 进行 sha256 加密：

```python
import hashlib

h = hashlib.new('sha256')  
h.update(b"Python")  
print(h.hexdigest())
```

**通过 `new()` 构造并指定一个哈希算法构造器，然后通过 `update()` 方法传入数据，最后通过 `hexdigest()` 方法即可获取对应的哈希数据。**

**需要注意，`update()` 方法支持的是二进制数据，用户传入的数据都需要进行二进制化。**

**同时，重复调用 `update()` 方法，相当于单次调用并传入所有参数的拼接结果，比如 `updata(a); updata(b)` 就相当于 `updata(a+b)`。**

> 对于字符数据的二进制化，可以直接在字符串前添加 b 进行自动转换，如 `b'Python'`。

> 对于数值数据的二进制化，可以使用 `bin()` 方法进行自动转换，如 `bin(10)`。

上述代码可以简写为：

```python
import hashlib

hexdata = hashlib.new('sha256', b"Python").hexdigest()
print(hexdata)
```

**`new()` 方法的第一个参数是哈希算法类型，第二个参数是目标数据，第二个可以省略。**

**特别的，hashlib 提供一些常用哈希算法的命名构造器，这些构造器的执行速度会比 `new()` 方法更快，比如：`md5(), sha1(), sha256()`。**

```python
import hashlib

hexdata = hashlib.md5(b'python').hexdigest()
print(hexdata)
```

# 对文件哈希

**在 Python 版本 3.11 后**，hashlib 模块提供 `file_digest()` 方法用于文件或文件型对象的高效哈希操作。

`file_digest()` 接收一个以二进制格式打开的文件对象，比如：

```python
import hashlib  
  
with open('requirements.txt', 'rb') as file:  
    digest = hashlib.file_digest(file, 'md5')  
    print(digest.hexdigest())
```

**特别的，如果是在版本 3.11 前**，则需要对文件分块读取后，将对应的数据流式传输给 `update()` 方法，逐步更新哈希值。

```python
import hashlib  
  
with open('requirements.txt', 'rb') as file:  
    digest = hashlib.new('md5')  
    chunk = file.read(2048)  
    while chunk:  
        digest.update(chunk)  
    print(digest.hexdigest())
```

# 拓展阅读：加密算法 SHA256

SHA256 的全称是 Secure Hash Algorithm 256，一种常用的加密算法，是 SHA-2 族算法中的一个，其它的还是 SHA222、SHA512 等。

Secure Hash Algorithm 256 是意思是：

- Secure 是指算法的输入输出一一对应，且不可逆（即只有编码而没有解码）
- Hash Algorithm 指的是散列算法，将一个任意长度的输入数据转换成固定长度的输出
- 256 是输出结果的位数，这个输出结果又被称为 Hash 值或者摘要

————————————

[ hashlib --- 安全哈希与消息摘要 - 官方文档 ](https://docs.python.org/zh-cn/3.13/library/hashlib.html)
