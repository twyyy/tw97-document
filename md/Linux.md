# UBUNTU 服务器版 安装流程

ubuntu中文站：https://cn.ubuntu.com/

本次安装系统版本：20.04.3

## 选择系统语言环境

默认语言环境为英语，选择默认即可

![image-20211219014126266.png](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219014126266.png)

## 版本更新

如果需要更新版本选择Update to the new installer

无需更新则选择Continue without updating

返回上一步选择Back

本次安装选择更新（选择更新后无法回退，请谨慎操作）

![image-20211219014644937](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219014644937.png)

## 选择键盘布局

默认键盘布局为英语（美国），使用默认设置即可

选择Done进入到下一步

![image-20211219015514900](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219015514900.png)

## IP地址设置

默认为动态IPv4地址，本次安装使用静态IPv4地址

选择编辑IPv4，如下图

![image-20211219020329900](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219020329900.png)

选择手动，如下图

![image-20211219020428970](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219020428970.png)

设置IPv4，如下图

![image-20211219021010754](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219021010754.png)

## 设置代理服务器

一般不需要设置代理服务器，直接下一步

![image-20211219021123926](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219021123926.png)

## 设置镜像源

本次安装已改为阿里镜像源 https://mirrors.aliyun.com/ubuntu

![image-20211219152812097](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219152812097.png)

## 磁盘分区

默认为单磁盘不分区,本次安装选择默认（括号中显示X表示选中,使用空格键选中）

![image-20211219022302567](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219022302567.png)

确认文件系统配置，可重置磁盘分区配置（Reset），本次安装直接选择下一步，如下图

![image-20211219022642072](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219022642072.png)

磁盘即将格式化，将会提示操作不可逆等警告，选择Continue继续，如下图

![image-20211219022845642](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219022845642.png)

## 账户设置

设置计算机名及登录账户（root账户为默认存在的，此处不能设置账户为root）,如下图

![image-20211219153308054](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219153308054.png)

## SSH设置

如果不需要远程连接则直接跳过此步,本次安装需要使用远程连接,选中Install OpenSSH server

Import SSH identity选择默认No

![image-20211219153355726](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219153355726.png)

## 系统服务安装清单

本次安装直接跳过此步

![image-20211219153620219](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219153620219.png)

## 安装完成

选择Reboot now重启系统完成安装

![image-20211219154046611](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219154046611.png)

## 使用root账户登录

先使用安装系统时设置的账户登录进入系统

输入以下命令修改root账户的密码

```shell
sudo passwd
```

输入命令后如下图（当前账户密码如果已输入过，则不会再要求输入）

![image-20211219160540282](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219160540282.png)

修改成功后退出当前账户登录，重新使用root账户登录即可

## 允许root账户远程连接

使用如下命令打开文件

```shell
vi /etc/ssh/sshd_config
```

打开文件后输入i编辑文件

找到#PermitRootLogin prohibit-password，修改为PermitRootLogin yes,修改完成后按ESC取消编辑,输入:wq保存并退出编辑

使用以下命令重启ssh服务

```shell
/etc/init.d/ssh restart
```

## 系统防火墙

### 开启防火墙

```shell
ufw enable
```

### 开机启动防火墙

```shell
ufw default deny
```

### 开启远程端口

```shell
ufw allow [远程端口号]
```

### 重启防火墙

```shell
ufw reload
```

# CentOS安装流程

CentOS官网：https://www.centos.org/

本次安装版本：8.5.2111

## 开始安装

选择Test this media & Install CentOS Linux 8开始安装

![image-20211225142010403](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225131225281.png)

## 语言选择

为了防止出现问题，实际生产环境建议使用英文

本次安装选择简体中文（中国）

![image-20211225142406710](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225131551426.png)

## 网络和主机名

网络设置完成后可能会卡顿一段时间，这是因为网络连通之后安装源开始自动获取数据

![image-20211225142550579](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225132935356.png)

![image-20211225142755597](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225133259672.png)

### 静态IP设置

![image-20211225142938633](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225134537922.png)

## 时间和日期

![image-20211225143415271](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225132429934.png)

![image-20211225143633907](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225143633907.png)

### 网络时间

添加cn.pool.ntp.org为NTP服务器

![image-20211225143805718](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225143805718.png)

打开“网络时间”开关

## 安装源

![image-20211225143937221](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225143937221.png)

本次安装使用阿里云镜像源：https://mirrors.aliyun.com/centos

![image-20211225144056932](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144056932.png)

## 安装目的地

![image-20211225144157836](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144157836.png)

存储配置，嫌麻烦可以默认自动

本次安装选择自定义

![image-20211225144301065](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144301065.png)

![image-20211225144607131](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144438129.png)

![image-20211225145011544](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145011544.png)

![image-20211225145059860](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144722852.png)

![image-20211225145238909](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145238909.png)

![image-20211225145402136](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225144811302.png)

![image-20211225145458347](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145458347.png)

## 软件选择

本次安装选择最小化安装

![image-20211225145534595](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145534595.png)

![image-20211225145711872](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145711872.png)

## 根密码

![image-20211225145801808](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145801808.png)

![image-20211225145857346](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225145857346.png)

## 开始安装

![image-20211225150035711](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225150035711.png)

## 完成安装

![image-20211225150702692](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211225150702692.png)

