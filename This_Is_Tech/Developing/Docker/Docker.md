---
tags:
  - Docker
icon: SiDocker
---

# Docker

> [!abstract] 本文摘要
> 本文主要介绍一些Docker常用的命令，便于速查速记

> [!info] 官方信息
> - [Docker: Accelerated Container Application Development](https://www.docker.com/)
> - [Docker Docs](https://docs.docker.com/)

> [!info] 社区资源
> [Docker -- 从入门到实践](https://docker-practice.github.io/zh-cn/)

---

| 命令                    | 说明                                 | 实例                                         |
| :-------------------- | ---------------------------------- | ------------------------------------------ |
| `docker run`          | 启动一个新的容器并运行命令                      | `docker run -d ubuntu`                     |
| `docker ps`           | 列出当前正在运行的容器                        | `docker ps`                                |
| `docker ps -a`        | 列出所有容器（包括已停止的容器）                   | `docker ps -a`                             |
| `docker build`        | 使用 Dockerfile 构建镜像                 | `docker build -t my-image .`               |
| `docker images`       | 列出本地存储的所有镜像                        | `docker images`                            |
| `docker pull`         | 从 Docker 仓库拉取镜像                    | `docker pull ubuntu`                       |
| `docker push`         | 将镜像推送到 Docker 仓库                   | `docker push my-image`                     |
| `docker exec`         | 在运行的容器中执行命令                        | `docker exec -it container_name bash`      |
| `docker stop`         | 停止一个或多个容器                          | `docker stop container_name`               |
| `docker start`        | 启动已停止的容器                           | `docker start container_name`              |
| `docker restart`      | 重启一个容器                             | `docker restart container_name`            |
| `docker rm`           | 删除一个或多个容器                          | `docker rm container_name`                 |
| `docker rmi`          | 删除一个或多个镜像                          | `docker rmi my-image`                      |
| `docker logs`         | 查看容器的日志                            | `docker logs container_name`               |
| `docker inspect`      | 获取容器或镜像的详细信息                       | `docker inspect container_name`            |
| `docker exec -it`     | 进入容器的交互式终端                         | `docker exec -it container_name /bin/bash` |
| `docker network ls`   | 列出所有 Docker 网络                     | `docker network ls`                        |
| `docker volume ls`    | 列出所有 Docker 卷                      | `docker volume ls`                         |
| `docker-compose up`   | 启动多容器应用（从 `docker-compose.yml` 文件） | `docker-compose up`                        |
| `docker-compose down` | 停止并删除由 `docker-compose` 启动的容器、网络等  | `docker-compose down`                      |
| `docker info`         | 显示 Docker 系统的详细信息                  | `docker info`                              |
| `docker version`      | 显示 Docker 客户端和守护进程的版本信息            | `docker version`                           |
| `docker stats`        | 显示容器的实时资源使用情况                      | `docker stats`                             |
| `docker login`        | 登录 Docker 仓库                       | `docker login`                             |
| `docker logout`       | 登出 Docker 仓库                       | `docker logout`                            |

常用选项：

- `-d`：后台运行容器，例如 `docker run -d ubuntu`
- `-it`：以交互式终端运行容器，例如 `docker exec -it container_name bash`
- `-t`：为镜像指定标签，例如 `docker build -t my-image .`
