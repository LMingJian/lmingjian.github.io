---
title: Linux—使用公钥私钥进行加解密
date: 2024-09-29
author: LM
---

## 1.使用 OpenSSL 进行非对称加解密

### a.非对称 RSA 密钥生成

```shell
# 生成私钥
openssl genpkey -algorithm RSA -out private_key.pem
# 生成公钥
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
### b.非对称加密

```shell
# 使用公钥加密
openssl rsautl -encrypt -pubin -inkey public_key.pem -in plaintext.txt -out encrypted.txt
# 使用私钥加密
openssl rsautl -encrypt -inkey private_key.pem -in plaintext.txt -out encrypted.txt
# 将输入内容直接进行公钥加密
echo $ONLYKEY | openssl rsautl -encrypt -pubin -inkey public.pem -out encrypted_key
```

### c.非对称解密

```shell
# 使用私钥解密
openssl rsautl -decrypt -inkey private_key.pem -in encrypted.txt -out decrypted.txt
# 使用公钥解密
openssl rsautl -decrypt -pubin -inkey public_key.pem -in encrypted.txt -out decrypted.txt
```

