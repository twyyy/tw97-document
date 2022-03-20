# Windows系统相关

## 压缩包安装MySQL

下载地址：https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.27-winx64.zip

本次安装MySQL版本：8.0.27

### 解压缩

将MySQL压缩包解压到需要存放的目录中

解压后的结构如下图

![image-20211219235004944](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219235004944.png)

### 初始化MySQL

使用管理员身份运行命令行工具

输入以下命令将命令行工具的执行目录切换到MySQL的bin目录下（以下命令中的C:\MySQL自行修改为MySQL的解压目录）

```shell
cd C:\MySQL\bin
```

输入以下命令初始化MySQL（命令执行过程中会打印一些内容在命令行工具中，其中包含root账户的密码，此处需要记录）

```shell
mysqld --initialize --console
```

打印的root账户密码如下图所示“Feu%MQsN_0h!”

![image-20211220000458967](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211220000458967.png)

### 安装MySQL服务

输入以下命令安装MySQL服务（以下命令中的“MySQL”为服务名称，可以不填，默认为“mysql”）

```shell
mysqld --install MySQL
```

### my.ini配置文件

默认安装是没有使用任何配置文件，如果需要使用my.ini配置文件，在MySQL的解压根目录下创建一个文件my.ini即可

### 启动 / 停止 MySQL

启动MySQL（以下命令中的“MySQL”自行替换为安装MySQL服务时所使用的服务名称）

```shell
net start MySQL
```

停止MySQL（以下命令中的“MySQL”自行替换为安装MySQL服务时所使用的服务名称）

```shell
net stop MySQL
```

## MySQL安全加固

### 安装密码策略插件

输入MySQL命令安装密码策略插件

```mysql
install plugin validate_password soname 'validate_password.dll';
```

输入MySQL命令查看默认密码策略

```mysql
show variables like '%validate_password%';
```

|               参数名称               | 默认值 |                             说明                             |
| :----------------------------------: | :----: | :----------------------------------------------------------: |
|  validate_password_check_user_name   |   ON   |      密码可以与用户名相同(ON -> 可以 \| OFF -> 不可以)       |
|  validate_password_dictionary_file   |        |                    检查密码的字典文件路径                    |
|       validate_password_length       |   8    |                     密码长度不能少于x位                      |
|  validate_password_mixed_case_count  |   1    |             密码中不能少于x个小写字母和大写字母              |
|    validate_password_number_count    |   1    |                    密码中不能少于x个数字                     |
|       validate_password_policy       | MEDIUM | 密码验证强度(可以使用数字0 / 1 / 2,也可以使用字符LOW / MEDUIM / STRONG) |
| validate_password_special_char_count |   1    |                  密码中不能少于x个特殊字符                   |

**密码验证强度说明：**

LOW: 检查长度

MEDUIM: 包含LOW的检查,另外检查数字个数 / 大小写字母个数 / 特殊字符个数

STRONG: 包含MEDUIM的检查,另外检查字典文件

### 安装登录策略插件

输入MySQL命令安装登录策略插件

```mysql
install plugin connection_control soname 'connection_control.dll';
install plugin connection_control_failed_login_attempts soname 'connection_control.dll';
```

输入MySQL命令查看默认登录策略

```mysql
show variables like '%connection_control%';
```

|                    参数名称                     |   默认值   |             说明             |
| :---------------------------------------------: | :--------: | :--------------------------: |
| connection_control_failed_connections_threshold |     3      |   失败尝试次数(0表示关闭)    |
|     connection_control_max_connection_delay     | 2147483647 | 最大延迟登录时间(单位: 毫秒) |
|     connection_control_min_connection_delay     |    1000    | 最小延迟登录时间(单位: 毫秒) |

# Linux系统相关

## 编译安装MySQL

本次安装MySQL版本：8.0.27

下载地址（标准版）：https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.27.tar.gz

下载地址（boost版）：https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-boost-8.0.27.tar.gz

### 解压缩

使用以下命令对MySQL源码压缩包解压

```shell
tar -zxvf [MySQL源码压缩包路径]
```

### 依赖安装

使用包管理器安装以下依赖（本次安装使用root账户，请根据实际情况判断是否需要使用超级管理员权限）

|         依赖包          |               版本要求                |
| :---------------------: | :-----------------------------------: |
|          cmake          |                                       |
|          make           |                > 3.75                 |
|           g++           |                                       |
|           gcc           | 如需要使用c++17的功能，安装版本 > 7.1 |
|       libssl-dev        |                                       |
|     libncurses-dev      |                                       |
|       pkg-config        |                                       |
|   gcc-toolset-10-gcc    |                                       |
| gcc-toolset-10-gcc-c++  |                                       |
| gcc-toolset-10-binutils |                                       |
|      libtirpc-dev       |                                       |
|      rpcsvc-proto       |                                       |

#### Ubuntu

```shell
apt -y install cmake make gcc g++ libssl-dev libncurses-dev pkg-config gcc-toolset-10-gcc gcc-toolset-10-gcc-c++ gcc-toolset-10-binutils
libtirpc-dev rpcsvc-proto
```

#### CentOS8

由于dnf包管理器中不包含rpcsvc-proto，此依赖需要单独安装

rpcsvc-proto下载地址：https://github.com/thkukuk/rpcsvc-proto/releases/download/v1.4.2/rpcsvc-proto-1.4.2.tar.xz

```shell
dnf -y install cmake make gcc gcc-c++ openssl-devel ncurses-devel pkg-config gcc-toolset-10-gcc gcc-toolset-10-gcc-c++ gcc-toolset-10-binutils libtirpc-devel
```

### 构建自动化编译脚本

使用以下命令构建自动化编译脚本

执行命令前注意查看执行目录下是否存在CMakeCache文件，如果存在先将此文件删除再执行命令

参数列表（构建时根据自身情况设定参数）

|          参数          |                             说明                             |
| :--------------------: | :----------------------------------------------------------: |
| -DCMAKE_INSTALL_PREFIX |                        MySQL安装目录                         |
|    -DMYSQL_DATADIR     |                        MySQL数据目录                         |
|    -DMYSQL_TCP_PORT    |                       端口号。默认3306                       |
|    -DDOWNLOAD_BOOST    | 是否下载boost库。如果安装标准版并且没有独立安装boost，则此项设置为1 |
|      -DWITH_BOOST      | boost库路径。<br/>如果选择下载boost库，则此项表示boost库的安装位置。<br/>如果不选择下载boost库，则此项表示本地boost库的路径，boost版本的安装包在根目录下可以找到boost |
| -DFORCE_INSOURCE_BUILD | CMAKE并不建议将构建时产生的文件生成在源代码目录下。<br/>如果需要强制构建在源代码目录，则此项设置为1<br/>如果需要构建在其他目录，则在需要存放构建文件的目录下执行以下命令即可 |

```shell
cmake [MySQL解压后的根目录] [参数...]
```

### 编译并安装

在构建cmake的目录下使用以下命令编译并安装

```shell
make -j [并行编译的CPU数量] && make install
```

### 创建配置文件

可以在以下几个位置创建配置文件my.cnf，优先级依次提升

/etc

/etc/mysql

~/

配置文件内容可以参考本文档中的“配置内容参考”

### 初始化数据库

使用以下命令初始化MySQL数据库

注意输出的内容中包含root账户的密码 root@localhost：密码

```shell
[mysql安装目录]/bin/mysqld --initialize --console
```

### 使用systemctl管理MySQL服务

#### 创建服务文件

```shell
> /usr/lib/systemd/system/[服务名称].service
```

#### 服务文件简单配置

[Unit]

Description=[服务说明]

[Service]

ExecStart=[mysql安装根目录]/bin/mysqld  --defaults-file=[配置文件路径，如果使用MySQL能够自动识别的路径，则可以不配置此项] --user=root

[Install]

WantedBy=multi-user.target

#### 重新加载systemctl

```shell
systemctl daemon-reload
```

#### 启动MySQL

```shell
systemctl start [MySQL服务名称]
```

#### 设置开机启动MySQL服务

```shell
systemctl enable [MySQL服务名称]
```

### 设置环境变量

使用以下命令打开文件

```shell
vi /etc/profile
```

按i键编辑文件，在文件末尾加入以下内容

export PATH="$PATH:[mysql安装目录]/bin"

按ESC键退出编辑，输入:wq保存并退出

使用以下命令重新加载环境变量设置

```shell
source /etc/profile
```

### 开启远程端口

#### Ubuntu

使用以下命令查询MySQL远程端口是否开启

如果命令执行结果中的Status不为active，表示防火墙没有开启，任何端口可通过

如果命令执行结果中的Status为active，表示防火墙开启，在status下方会打印出开放的端口号

```shell
ufw status
```

使用以下命令开启MySQL远程端口

```shell
ufw allow [MySQL远程端口号]
```

重启防火墙

```shell
ufw reload
```

#### CentOS8

使用以下命令查询MySQL远程端口是否开启

```shell
firewall-cmd --query-port=[mysql端口号]/tcp
```

如果MySQL远程端口未开启，输入以下命令开启MySQL远程端口

```shell
firewall-cmd --add-port=[mysql端口号]/tcp --permanent
```

重新加载防火墙设置

```shell
firewall-cmd --reload
```

# 配置内容参考

根据实际安装情况调整部分参数的值

```ini
[mysqld]
# 远程端口
port=3306
# 字符集排序规则
collation-server="utf8mb4_general_ci"
# 数据存放目录
datadir="此项需要更改"
# 慢查询日志是否开启
slow-query-log=1
# 导入导出目录
secure-file-priv="此项需要更改"
# 最大并发客户端连接数
max_connections=2000
# 最大中断请求次数
max_connect_errors=4000
# 缓存池大小
innodb_buffer_pool_size=4G
# 脏页占缓存池的比例，达到此比例刷脏页到磁盘
innodb_max_dirty_pages_pct=30
# 交互等待时间
interactive_timeout=500
# 非交互等待时间
wait_timeout=500
# 每个日志文件的大小限制
innodb_log_file_size=256M
# 每个日志文件的大小限制
innodb_flush_log_at_trx_commit=2
```

# 常用命令

## 修改密码

```mysql
alter user '[用户名]'@'[主机地址]' identified by '[密码]';
```

## 刷新权限

```mysql
flush privileges;
```

## 创建用户

```mysql
create user '[用户名]'@'[主机地址]' identified by '[密码]';
```

## 授权/取消授权

```mysql
[grant/revoke] [权限] on [数据库/表] to '[用户名]'@'[主机地址]';
```

## 删除用户

```mysql
drop user '[用户名]'@'[主机地址]';
```

## 删除字段

### 删除单个字段

```mysql
alter table [表名] drop [字段名]
```

### 批量删除字段

```mysql
alter table [表名] drop [字段名1],drop [字段名2]...drop [字段名N]
```

## 修改字段

```mysql
alter table [表名] change [原字段名] [新字段名] [数据类型(长度)] [是否为空] [自增] [主键] [字段备注];
```

是否为空：填入null表示字段可以为空，填入not null表示字段不能为空，如果不填则默认字段可以为空

自增：填入auto_increment表示字段自增，如果不填则默认字段不自增

主键：填入primary key表示字段为主键，如果不填则默认字段不为主键

字段备注：填入 “comment '备注内容'” 设置字段备注，如果不填则默认备注为空

## 插入字段

```mysql
alter table [表名] add [新字段名] [数据类型(长度)] [是否为空] [自增] [主键] [字段备注] [跟随字段];
```

是否为空、自增、主键、字段备注 已在“修改字段”中做相关说明，此处不在叙述

跟随字段：填入 “after [字段名]” 设置新插入的字段跟随在哪个字段之后，如果不填则在末尾插入新字段

# 权限列表

|        **权 限**        |     **作用范围**     |           **作 用**           |
| :---------------------: | :------------------: | :---------------------------: |
|           all           |        服务器        |           所有权限            |
|         select          |        表、列        |            选择行             |
|         insert          |        表、列        |            插入行             |
|         update          |        表、列        |            更新行             |
|         delete          |          表          |            删除行             |
|         create          |   数据库、表、索引   |             创建              |
|          drop           |   数据库、表、视图   |             删除              |
|         reload          |        服务器        |       允许使用flush语句       |
|        shutdown         |        服务器        |           关闭服务            |
|         process         |        服务器        |         查看线程信息          |
|          file           |        服务器        |           文件操作            |
|      grant option       | 数据库、表、存储过程 |             授权              |
|       references        |      数据库、表      |        外键约束的父表         |
|          index          |          表          |         创建/删除索引         |
|          alter          |          表          |          修改表结构           |
|     show databases      |        服务器        |        查看数据库名称         |
|          super          |        服务器        |           超级权限            |
| create temporary tables |          表          |          创建临时表           |
|       lock tables       |        数据库        |             锁表              |
|         execute         |       存储过程       |             执行              |
|   replication client    |        服务器        | 允许查看主/从/二进制日志状态  |
|    replication slave    |        服务器        |           主从复制            |
|       create view       |         视图         |           创建视图            |
|        show view        |         视图         |           查看视图            |
|     create routine      |       存储过程       |         创建存储过程          |
|      alter routine      |       存储过程       |       修改/删除存储过程       |
|       create user       |        服务器        |           创建用户            |
|          event          |        数据库        |    创建/更改/删除/查看事件    |
|         trigger         |          表          |            触发器             |
|    create tablespace    |        服务器        | 创建/更改/删除表空间/日志文件 |
|          proxy          |        服务器        |       代理成为其它用户        |
|          usage          |        服务器        |           没有权限            |

# 主从复制

将主库锁定,禁止主库任何数据操作行为

同步主库与从库的数据结构以及数据

主库中创建主从复制专用账户

主库中授予专用账户主从复制权限并刷新权限

主库中输入MySQL命令查看主库ID (记住这个ID,配置从库时将会使用)

```mysql
show variables like 'server_id';
```

主库中输入MySQL命令查看主库状态 (记住结果中的File和Position的值)

```mysql
show master status;
```

从库配置文件[mysqld]节点下添加server_id参数 (参数值不能和主库查询的server_id相同)

如果主从复制操作有需要忽略的库或者指定的库,可以添加以下配置在从库配置文件的[mysqld]节点下

|      配置名称       |                           配置说明                           |
| :-----------------: | :----------------------------------------------------------: |
| replicate-ignore-db |        不需要同步的数据库名称,多个库用英文逗号","隔开        |
|   replicate-do-db   | 需要同步的数据库名称,多个库用英文逗号","隔开 (如果未配置此项表示同步专用账户拥有权限的所有库) |

从库中输入MySQL命令配置远程同步账户与二进制节点

```mysql
change master to master_host="[主库主机地址]",master_user="[主从复制账户]",master_password="[主从复制账户密码]",master_log_file="[主库状态File]",master_log_pos=[主库状态Position];
```

从库中执行MySQL命令开始复制

```mysql
start slave;
```

从库中输入命令查看从库状态

```mysql
show slave status;
```

将从库状态显示的结果拷贝到文本编辑器中,搜索error关键字,如果未查询到有具体错误,则表示主从复制一切正常
