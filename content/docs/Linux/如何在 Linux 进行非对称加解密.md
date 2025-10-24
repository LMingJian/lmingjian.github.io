---
title: 如何在 Linux 进行非对称加解密
date: 2024-12-25T13:55:36+08:00
author: LiangMingJian
---

# 概述

非对称加密是一种使用公钥和私钥进行加密与解密的密码学方法，其核心特点是加密和解密的过程中需要使用不同的密钥。

- **公钥加密**的文件必须用**私钥解密**
- **私钥加密**的文件必须用**公钥解密**

在 Linux 中，用户可以使用 OpenSSL 进行非对称加密。

# 非对称 RSA 密钥生成

**生成私钥**

```bash
openssl genpkey -algorithm RSA -out private_key.pem
# genpkey：生成私钥
# -algorithm RSA：指定 RSA 算法
# -out private_key.pem：输出为指定文件
```

**生成公钥**

```bash
openssl rsa -pubout -in private_key.pem -out public_key.pem
# rsa：使用 RSA 算法
# -pubout：生成公钥
# -in private_key.pem：需要使用的私钥
# -out public_key.pem：输出的公钥文件
```

# 非对称加解密

**公钥加密，私钥解密**

```shell
# 使用公钥加密
openssl pkeyutl -encrypt -pubin -inkey public_key.pem -in plaintext.txt -out encrypted.txt
# pkeyutl：通用的非对称加密解密工具（旧版是 rsautl）
# -encrypt：指定加密操作
# -pubin：指定使用的是公钥（不传入该参数时默认使用私钥）
# -inkey public_key.pem：公钥的地址
# -in plaintext.txt：需要加密的文件
# -out encrypted.txt：输出的文件

# 使用私钥解密
openssl pkeyutl -decrypt -inkey private_key.pem -in encrypted.txt -out decrypted.txt
# -decrypt：指定解密操作
```

**私钥加密，公钥解密**

```shell
# 使用私钥加密
openssl pkeyutl -encrypt -inkey private_key.pem -in plaintext.txt -out encrypted.txt

# 使用公钥解密
openssl pkeyutl -decrypt -pubin -inkey public_key.pem -in encrypted.txt -out decrypted.txt
```

**特别的，直接对输入内容而不是文件进行加密**

```shell
# 将输入内容直接进行公钥加密
echo $ONLYKEY | openssl pkeyutl -encrypt -pubin -inkey public.pem -out encrypted_key
# 将加密文件解密为输入内容
openssl pkeyutl -decrypt -inkey private.pem -in encrypted_key
```
