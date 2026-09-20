---
title: 如何导出 Docker 镜像
date: 2024-12-25T11:13:16+08:00
author: LiangMingJian
---

# 镜像的导出

使用 `save` 命令可以将**镜像**打包成压缩文件。

```bash
# docker save 镜像名:标签 -o 导出文件.tar
docker save my_images:v1.0 -o my_images_output.tar
```

> 注意：如果使用镜像 ID 打包，那么当镜像在另一台服务器加载时镜像名字和 Tag 版本都会为 none，此时需要使用 `docker tag` 来重命名镜像。

特别的，如果想要导入多个镜像，那么可以将多个镜像名一并写入然后导出。

```bash
docker save my_images_1:v1.0 my_images_2:v1.0 -o my_images_output.tar
```

最后，如果想一次性导出系统上所有的镜像，可以通过 `docker images` 命令查询，然后使用 `$()` 写入导出命令完成导出。

```bash
docker save $(docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "<none>") -o my_images_output.tar
```

# 镜像的导入

使用 `load` 命令可以将文件导入回镜像库中。

```bash
# docker load -i 镜像导出文件.tar
docker load -i my_images_output.tar
```

——————————————————————————————

# 容器的导出

除了上述镜像的导出外，Docker 还支持导出容器（当用户修改了容器内文件时，此时可以导出容器保存这些修改）。

使用 `export` 命令，可以导出正在运行的容器。

```bash
# docker export 容器名 -o 导出文件.tar
docker export my_container -o my_container_output.tar
```

# 容器的导入

特别的，容器的导出文件不能被重新导入为一个容器，只能通过 `import` 命令导入为一个新的镜像，然后再重新运行为容器。

```bash
# docker import 导出文件.tar 镜像名:标签
docker import my_container_output.tar new_images:v1.0
```

# 容器的提交 commit

实际上，对容器的修改一般建议通过 `docker commit` 命令提交到镜像来进行保存，不建议通过导出容器来进行保存。

```bash
# docker commit -a 作者 -m 提交信息 容器名 镜像名:标签
docker commit -a="my" -m="my commit" my_container my_images:v1.0
```
