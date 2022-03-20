# Linux系统相关

## 编译安装Redis

### 解压缩

使用以下命令对Redis源码压缩包解压

```shell
tar -zxvf [Redis源码压缩包路径]
```

### 依赖安装

使用包管理器安装以下依赖（本次安装使用root账户，请根据实际情况判断是否需要使用超级管理员权限）

| 依赖包 |               版本要求                |
| :----: | :-----------------------------------: |
|  make  |                > 3.75                 |
|  gcc   | 如需要使用c++17的功能，安装版本 > 7.1 |

#### Ubuntu

```shell
apt -y install make gcc
```

#### CentOS8

```shell
dnf -y install make gcc
```

### 使用systemctl管理Redis服务

#### 创建服务文件

```shell
> /usr/lib/systemd/system/[服务名称].service
```

#### 服务文件简单配置

[Unit]

Description=[服务说明]

[Service]

ExecStart=[Redis安装根目录]/bin/redis-server [配置文件路径]

[Install]

WantedBy=multi-user.target

#### 重新加载systemctl

```shell
systemctl daemon-reload
```

#### 启动Redis

```shell
systemctl start [Redis服务名称]
```

#### 设置开机启动Redis服务

```shell
systemctl enable [Redis服务名称]
```

### 设置环境变量

使用以下命令打开文件

```shell
vi /etc/profile
```

按i键编辑文件，在文件末尾加入以下内容

export PATH="$PATH:[Redis安装目录]/bin"

按ESC键退出编辑，输入:wq保存并退出

使用以下命令重新加载环境变量设置

```shell
source /etc/profile
```

### 开启远程端口

#### Ubuntu

使用以下命令查询Redis远程端口是否开启

如果命令执行结果中的Status不为active，表示防火墙没有开启，任何端口可通过

如果命令执行结果中的Status为active，表示防火墙开启，在status下方会打印出开放的端口号

```shell
ufw status
```

使用以下命令开启Redis远程端口

```shell
ufw allow [Redis远程端口号]
```

重启防火墙

```shell
ufw reload
```

#### CentOS8

使用以下命令查询Redis远程端口是否开启

```shell
firewall-cmd --query-port=[Redis端口号]/tcp
```

如果Redis远程端口未开启，输入以下命令开启Redis远程端口

```shell
firewall-cmd --add-port=[Redis端口号]/tcp --permanent
```

重新加载防火墙设置

```shell
firewall-cmd --reload
```
