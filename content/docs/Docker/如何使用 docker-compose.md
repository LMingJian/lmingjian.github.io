---
title: 如何使用 docker-compose
date: 2025-05-22T19:21:39+08:00
author: LiangMingJian
---

# 使用 docker-compose 配置 host 网络

```yml
version: '3.8'

services:
  web:
    image: nginx:latest
    network_mode: host
```
