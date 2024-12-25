---
title: 如何从 Docker 容器里复制文件
date: 2024-12-25T11:13:11+08:00
author: LiangMingJian
---

# 文件复制

通过`cp`命令进行文件的复制。

```bash
docker cp testtomcat：/usr/local/tomcat/webapps/test/js/test.js /opt  
# 从容器里面拷文件到宿主机
docker cp /opt/test.js testtomcat：/usr/local/tomcat/webapps/test/js  
# 从宿主机拷文件到容器里面
```
