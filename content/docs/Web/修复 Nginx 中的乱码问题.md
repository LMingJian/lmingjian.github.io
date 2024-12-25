---
title: 修复 Nginx 中的乱码问题
date: 2024-12-25T16:16:10+08:00
author: LiangMingJian
---

# 问题

当 Nginx 服务器运行时，有时用户访问中文内容会出现乱码。

# 解决

需要修改 Nginx 的 server 配置内容，增加字段：`charset utf-8;`。

```
server {
    listen   80;
    server_name you.example.com;
    charset utf-8;
    
    location /examples {
        return 403;
    }
}
```
