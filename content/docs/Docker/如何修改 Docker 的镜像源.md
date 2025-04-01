---
title: 如何修改 Docker 的镜像源
date: 2024-12-25T11:13:44+08:00
author: LiangMingJian
---

# 修改配置文件

创建或修改 `/etc/docker/daemon.json` 文件，填入以下内容。

```json
{
     "registry-mirrors": ["https//hub-mirror.c.163.com"]
}
```

修改完成后，重启 Docker 服务。

```
systemctl daemon-reload
systemctl restart docker
```

- ~~Docker中国区官方镜像：https://registry.docker-cn.com~~
- ~~网易：http://hub-mirror.c.163.com~~
- ~~中国科技大学 ：https://docker.mirrors.ustc.edu.cn~~
- 上述镜像似乎已过期，不建议使用，请自行检查。

# 更新日志

> @2025-04-01
> https://cloud.tencent.com/developer/article/2485043
> 从以上地址获取。

[https://docker.xuanyuan.me]()

[https://docker.1ms.run]()

[https://dockers.xuanyuan.me]()

[https://dockerdocs.xuanyuan.me]()

[https://xdark.top]()
