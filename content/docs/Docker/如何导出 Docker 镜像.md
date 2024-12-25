---
title: 如何导出 Docker 镜像
date: 2024-12-25T11:13:16+08:00
author: LiangMingJian
---

# 镜像的导出备份

使用`save`命令可以将**镜像**打包成压缩文件。

注意：如果使用使用镜像 ID 打包，那么当镜像在另一台服务器加载时镜像名字和 Tag 版本都会为 none，此时需要使用`docker tag`来重命名镜像

```bash
docker save -o 镜像导出文件(格式为tar压缩文件) 镜像ID或镜像名称[:版本号]
docker save -o $(pwd)/mytomcat.tar newtomcat:v1.0
----------------------------------------------------------------------
docker save 镜像 ID 或镜像名称[:版本号] > 镜像导出文件
docker save newtomcat:v1.0 > $(pwd)/mytomcat.tar 
```

# 镜像的导入恢复

使用`load`命令可以将文件导入回镜像库中。

```bash
docker load -i 镜像导出文件(格式为tar压缩文件)
docker load -i $(pwd)/mytomcat.tar
--------------------------------------------------------------------
docker load < 镜像导出文件(格式为tar压缩文件)
docker load < $(pwd)/mytomcat.tar
```

# 容器的导出备份

使用`export`命令可以将正在运行的**容器**进行打包导出。

```bash
docker export -o 容器导出文件 容器ID或容器名称
docker export -o $(pwd)/newtomcat.tar mytomcat
-----------------------------------------------------
docker export 容器ID或容器名称 > 容器导出文件(格式为tar压缩文件) 
docker export mytomcat > $(pwd)/newtomcat.tar 
-----------------------------------------------------
注释：
$(pwd) 是 docker 支持的获取当前目录路径的方法，与 linux 的 pwd 类似
$(pwd)/newtomcat.tar 表示在当前目录下生成一个 newtomcat.tar 压缩文件
```

# 容器的导入恢复

使用`import`命令可以将文件导入成新的镜像，后续可以从这个镜像重建容器。

```bash
docker import 容器导出文件(格式为tar压缩文件) 新镜像名称[:版本号]
docker import $(pwd)/newtomcat.tar newtomcat:v1.0
------------------------------------------------------
docker import /URL 新镜像名称[:版本号]
docker import http://example.com/exampleimage.tgz example/imagerepo
```
