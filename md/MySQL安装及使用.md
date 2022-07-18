# 在线地址：<https://github.com/twyyy/tw97-document/tree/master/md>

## Docker安装

```shell
# 拉取最新镜像
docker pull mysql

# 创建并运行容器
docker run -d --name mysql --restart=always -p 3306:3306 -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 mysql
```

## 安装密码验证组件

```sql
-- 安装
INSTALL COMPONENT 'file://component_validate_password';

-- 卸载
UNINSTALL COMPONENT 'file://component_validate_password';

-- 查看已安装组件
SELECT * FROM mysql.component;

-- 查看相关配置
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

## 安装连接控制插件

```sql
-- linux安装
INSTALL PLUGIN CONNECTION_CONTROL SONAME 'connection_control.so';
INSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS SONAME 'connection_control.so';

-- windows安装
INSTALL PLUGIN CONNECTION_CONTROL SONAME 'connection_control.dll';
INSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS SONAME 'connection_control.dll';

-- 卸载
UNINSTALL PLUGIN CONNECTION_CONTROL;
UNINSTALL PLUGIN CONNECTION_CONTROL_FAILED_LOGIN_ATTEMPTS;

-- 查看已安装插件
SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE 'connection%';

-- 查看相关配置
show variables like 'CONNECTION_CONTROL%';
```

|配置名称|默认值|说明|
|---|---|---|
|connection_control_failed_connections_threshold|3|允许连续连接失败的次数（0表示不限制）|
|connection_control_max_connection_delay|默认：2147483647<br/>最小：1000<br/>最大：2147483647|连接失败最大响应延迟（单位：毫秒）|
|connection_control_min_connection_delay|默认：1000<br/>最小：1000<br/>最大：2147483647|连接失败最小响应延迟（单位：毫秒）|

## SQL分类

|全称|简称|说明|代表关键字|
|--|--|--|--|
|Data Query Language|DQL|数据查询语言|select|
|Data Manipulation Language|DML|数据操作语言|insert、update、delete|
|Data Definition Language|DDL|数据定义语言|create、drop、alter|
|Transaction Control Language|TCL|事务控制语言|begin、rollback、commit|
|Data Control Language|DCL|数据控制语言|grant、revoke|

## 笛卡尔积现象

```txt
如果两张表进行连接查询，在没有任何条件限制下，最终的查询结果数是两张表记录数的乘积

解决方法：连接两张表时做条件限制

说明：即使使用条件限制避免了笛卡尔积现象，查询出来的数据是有效的记录，但是匹配次数依旧不会减少
```

## 事务

```txt
在mysql中，默认情况下一条sql语句就是一个事务，不过mysql做了自动提交。
```

### 事务的特性

|全称|简称|中文表达|说明|
|-|-|-|-|
|Atomicity|A|原子性|事务是一个不可再分的工作单位，要么全部成功，要么全部失败|
|Consistency|C|一致性|在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏|
|Isolation|I|隔离性|多个事务并发访问时，事务之间是隔离的，一个事务不应该影响其它事务运行效果|
|Durability|D|持久性|事务一旦提交，该事务对数据库所作的更改便持久的保存在数据库之中|

### 事务的隔离级别

```txt
1. Read uncommitted（读未提交）
  说明：事务A可以读取到事务B未提交的数据
  问题：脏读现象

2. Read committed（读已提交）
  说明：事务A只能读取到事务B提交后的数据
  问题：不可重复读

3. Repeatable read（可重复读）（mysql默认）
  说明：无论事务开启了多久，每次读取到的数据都是事务开启时的数据，即使数据已经发生了变化
  问题：幻读现象

4. Serializable（序列化）
  说明：事务排队，不能并发
  问题：锁表
```

## 数据库设计范式

```txt
1. 第一范式：
  要求任何一张表都必须有主键，每一个字段原子性不可再分

2. 第二范式：
  要求所有非主键字段完全依赖主键，不能产生部分依赖

3. 第三范式：
  要求所有非主键字段直接依赖主键，不能产生传递依赖

以上皆为理论上的设计范式，在实际设计过程中可能会使用冗余换速度（即空间换时间）。
```
