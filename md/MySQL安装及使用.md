# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

## 安装

------

### Docker安装

#### 拉取最新镜像

```shell
docker pull mysql
```

#### 创建并运行容器

```shell
docker run -d --name mysql --restart=always -p 3306:3306 -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 mysql
```

------

## 安装密码验证组件

### 安装

```shell
INSTALL COMPONENT 'file://component_validate_password';
```

### 查看已安装组件

```shell
SELECT * FROM mysql.component;
```

### 查看相关配置

```shell
SHOW VARIABLES LIKE 'validate_password.%';
```

|配置名称|默认值|说明|
|---|---|---|
|validate_password.check_user_name|ON|ON：不允许密码与用户名相同或相反（区分大小写）<br/>OFF：允许密码与用户名相同或相反|
|validate_password.dictionary_file||检查密码的字典文件的路径|
|validate_password.length|8|密码的最小长度|
|validate_password.mixed_case_count|1|密码中小写字母和大写字母的个数|
|validate_password.number_count|1|密码中数字的个数|
|validate_password.policy|1|0(LOW)：只检查密码长度<br/>1(MEDIUM)：检查长度、大写和小写字母的个数、数字的个数、特殊字符的个数<br/>2(STRONG)：包含1的检查规则，额外检查字典文件|
|validate_password.special_char_count|1|密码中特殊字符的个数|

### 修改配置

在配置文件的 [mysqld] 节点下写入 [配置名称]=[值] 即可，重启mysql生效。

### 卸载

```shell
UNINSTALL COMPONENT 'file://component_validate_password';
```

------

## 安装连接控制插件

### linux安装

```shell
INSTALL PLUGIN CONNECTION_CONTROL SONAME 'connection_control.so';
INSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS SONAME 'connection_control.so';
```

### windows安装

```shell
INSTALL PLUGIN CONNECTION_CONTROL SONAME 'connection_control.dll';
INSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS SONAME 'connection_control.dll';
```

### 查看已安装插件

```shell
SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE 'connection%';
```

### 查看相关配置

```shell
show variables like 'CONNECTION_CONTROL%';
```

|配置名称|默认值|说明|
|---|---|---|
|connection_control_failed_connections_threshold|3|允许连续连接失败的次数（0表示不限制）|
|connection_control_max_connection_delay|默认：2147483647<br/>最小：1000<br/>最大：2147483647|连接失败最大响应延迟（单位：毫秒）|
|connection_control_min_connection_delay|默认：1000<br/>最小：1000<br/>最大：2147483647|连接失败最小响应延迟（单位：毫秒）|

### 修改配置

在配置文件的 [mysqld] 节点下写入 [配置名称]=[值] 即可，重启mysql生效。

### 卸载

```shell
UNINSTALL PLUGIN CONNECTION_CONTROL;
UNINSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS;
```

------
