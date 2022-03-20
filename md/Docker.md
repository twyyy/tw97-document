# 常用命令

## 拉取镜像

```shell
docker pull [镜像名称]
```

## 查看所有镜像

```shell
docker images
```

## 删除一个镜像

```shell
docker rmi [镜像ID]
```

## 删除所有镜像

```shell
docker rmi -f $(docker images -q)
```

## 构建镜像

```shell
docker build . -t [镜像名称]:[镜像版本号] -f Dockerfile
```

## 创建容器

```shell
docker create -p [映射端口]:[程序端口] -v [挂载文件或文件夹本地路径]:[容器文件或文件夹路径] --restart=always --name [容器名称] [镜像名称]:[镜像版本]
```

## 获取所有容器

```shell
docker ps -a
```

## 启动单个容器

```shell
docker start [容器名称]
```

## 启动所有容器

```shell
docker start $(docker ps -aq)
```

## 查看容器运行日志

```shell
docker logs -f [容器名称]
```

## 复制本地文件到容器中

```shell
docker cp [新文件路径] [容器名称]:[容器文件路径]
```

## 导出镜像

```shell
docker save -o [输出文件全名.tar] [镜像名称]
```

## 导入镜像

```shell
docker load < [文件全名]
```

## 查询容器信息

```shell
docker inspect [容器ID]
```

## 进入容器

```shell
docker exec -it [容器ID] /bin/bash
```

## 复制容器中的文件到本地

```shell
docker cp [容器ID]:[容器文件路径] [本地路径]
```

# 常用配置（示例）

```dockerfile
FROM [镜像]

COPY [项目文件路径] [容器文件路径]

WORKDIR [工作路径]

EXPOSE [端口/协议]

# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

ENTRYPOINT [ "启动命令","项目入口文件" ]
```

# Linux systemctl 配置docker守护程序（示例）

```ini
[Unit]

[Service]
Type=notify
ExecStart=[docker守护程序路径]

[Install]
WantedBy=multi-user.target
```

# Linux开启docker网络转发

修改 /etc/sysctl.conf 文件 net.ipv4.ip_forward 配置为 1

重启 network 服务，重启 docker 容器
