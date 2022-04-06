---
title: "Podman"
date: 2022-04-06T18:36:28+08:00
draft: false
---

podman使用笔记

<!--more-->

# podman

## 在CentOS8上安装podman

- 官方源已经纳入`dnf install podman -y` ，未经测试的版本安装。

    ```shell
    dnf -y module disable container-tools
    dnf -y install 'dnf-command(copr)'
    dnf -y copr enable rhcontainerbot/container-selinux
    curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_8/devel:kubic:libcontainers:stable.repo
    dnf -y install podman
    ```

- podman安装完成后只在目录/etc/cni/net.d/下有一个和Bridge相关的配置文件，以后每创建一个容器网络会生成一个对应的配置文件。同时安装时会安装依赖包containers-common，它包含很多i配置。
- 用于保存registries相关配置的/etc/containers/registries.conf
- 指定在执行`podman run`或者`podman build`命令时自动挂载的路径，该路径只会在容器运行时挂载，不会提交到容器镜像中，路径/usr/share/containers/mounts.conf。
- 证书相关的配置/etc/containers/policy.json
- 较新版本内核才能执行命令`sysctl kernel.unprivileged_userns_clone=1 && echo 'kernel.unprivileged_userns_clone=1' > /etc/sysctl.d/userns.conf`
- 添加镜像加速，修改文件/etc/containers/registries.conf为

    ```conf
    unqualified-search-registries = ["docker.io"]

    [[registry]]
    prefix = "docker.io"
    location = "xxx.mirror.aliyuncs.com"
    #location = "nd6egsul.mirror.aliyuncs.com"

    ```

- 或者修改文件/etc/containers/registries.conf中对应行为`registries = ['registry.redhat.io','quay.io','docker.io','registry.access.redhat.com']`

## 启动一个nextcloud容器

- 创建网络`podman network create -d bridge br-sgnay`
- 创建postgresql数据库，获取配置文件`podman run -i --rm postgres cat /usr/share/postgresql/postgresql.conf.sample > postgres/my-postgres.conf`，运行容器`podman --log-level=debug run -dt --name mypostgres -e POSTGRES_PASSWORD=pg792458 -v /home/data/containers/postgres/my-postgres.conf:/etc/postgresql/postgresql.conf -v /home/data/containers/postgres/data:/var/lib/postgresql/data -p 15432:5432 postgres -c 'config_file=/etc/postgresql/postgresql.conf'`。
- 创建nextcloud的数据库用户`CREATE USER nextcloud WITH PASSWORD 'nc792458';`，创建数据库`CREATE DATABASE nextcloud OWNER nextcloud;`，授权给用户数据库`GRANT ALL PRIVILEGES ON DATABASE nextcloud to nextcloud;`
- 执行`podman run -d --name mynextcloud -e POSTGRES_DB=nextcloud -e POSTGRES_USER=nextcloud -e POSTGRES_PASSWORD=nc792458 -e POSTGRES_HOST=mypostgres:15432 -p 1088:80 -v /home/data/containers/nextcloud/data:/var/www/html nextcloud`
- 使用www-data登入容器`podman exec -it --user www-data nextcloud /bin/bash`
- 允许外部存储`php occ app:enable files_external`
- 修改分片大小`php occ config:app:set files max_chunk_size --value 104857600`
- install fuse-overlayfs
- `chmod 4755 /usr/bin/newgidmap && chmod 4755 /usr/bin/newuidmap`
- systemd.unified_cgroup_hierarchy=1
- 手动同步文件到目录`sudo -u www-data php occ files:scan sgnay`

## 部署odoo仓管系统

- 创建pod`podman pod create --name pododoo -p 8069:8069`
- 创建数据库`podman run -d -v /data/postgresql/data:/usr/lib/postgresql/data -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db --pod pododoo postgres:10`
- 创建odoo`podman run -d -v /data/odoo/addons:/mnt/extra-addons --name odoo --pod pododoo odoo`

