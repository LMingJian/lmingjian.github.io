---
title: 如何从 Docker 容器里复制或映射文件
date: 2024-12-25T11:13:11+08:00
author: LiangMingJian
---

# 从容器里复制文件出来

在 Docker 中，通过 `cp` 命令可以复制文件。

> 从容器中复制文件时，容器必须处于运行中状态。

执行：`docker cp <容器>:<源路径> <本地路径>`。

**复制单个文件**

```bash
docker cp <容器名>:/usr/local/test.sh ./
```

**复制文件夹**

```bash
docker cp <容器名>:/usr/local/test ./
```

# 从宿主机复制文件到容器

这部分操作与从容器里复制相反，需要先写本地的文件路径，再写容器内的路径。

执行：`docker cp <本地路径> <容器>:<目标路径>`

**复制单个文件**

```bash
docker cp ./test.sh <容器名>:/usr/local/test.sh
```

**复制文件夹**

```bash
docker cp ./tset <容器名>:/usr/local/test 
```

# 文件映射

在日常使用中，往往我们不会选择上述方式修改容器内的文件，而是通过 `volumes` 进行文件映射。

在 `docker-compose.yml` 文件中，我们可以通过如下参数配置 `volumes` 映射。

```yml
version: '3.1'

services:
  database:
    image: database:8
    container_name: database
    restart: always
    ports:
      - "3306:3306"
    privileged: true
    tty: true
    volumes:
      - /home/platform/database:/var/lib/mysql
```

在 `volumes` 中，依据 `- <本地路径>:<容器路径>` 来编写配置。

注意，Docker 在启动容器并处理卷映射（`volumes`）时，会检查宿主机上的目标路径：

- 当宿主机中目标目录或文件**不存在**的时候（比如第一次运行），Docker 会在**宿主机上自动创建该目录**，并将‌容器内的文件自动复制到宿主机新创建的目录中。
- 当宿主机中目标目录或文件**已存在**的时候（比如再次运行），Docker 此时就不会复制容器内的文件，而是会**将宿主机的文件覆盖到容器**内。

特别的，**在较新版本中，Docker 不再尝试自动初始化内容**，以避免意外覆盖用户数据。

即在第一次运行时，**较新版本的 Docker 不再会将‌容器内的文件自动复制到宿主机的目录**中。

在新版本 Docker 中，依据官方文档 [Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/) 中所述：

> If you use `--volume` to bind-mount a file or directory that does not yet exist on the Docker host, Docker automatically creates the directory on the host for you. It's always created as a directory.（如果你使用 `--volume` 来绑定挂载一个在 Docker 主机上尚不存在的文件或目录，Docker 会自动在主机上为你创建该目录。它总是以目录的形式创建。）

> By default, `--mount` does not automatically create a directory if the specified mount path does not exist on the host. Instead, it produces an error.（默认情况下，如果主机上不存在指定的挂载路径，`--mount` 不会自动创建目录。相反，它会产生一个错误。）

Docker 容器在初始化 `volumes` 时，只会创建一个空白目录，如果容器内一个包含文件内容的文件夹被映射到主机，那么主机中最多会自动创建一个相同文件夹，但文件夹内会是空白。

因此，在这种情况下，Docker 的文件映射一般只适用于系统运行中生成文件的存放位置，比如映射数据库，图片，视频等资源文件。

如果确实需要映射一些不在系统运行时生成的文件，比如配置文件，那么就只能先暂时注释掉 `volumes` 配置，使用 `cp` 命令从容器中复制文件内容后再恢复 `volumes` 配置。
