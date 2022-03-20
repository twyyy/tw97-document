# Windows Server 2022系统安装流程

## 语言/时间/键盘 设置

如下图设置

![image-20211219173508425](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219173508425.png)

## 开始安装

点击现在安装即可开始安装

![image-20211219173650165](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219173650165.png)

## 系统激活

如果有产品秘钥则输入产品秘钥进入下一步

如果没有产品秘钥则点击“我没有产品秘钥”进入下一步

本次安装没有产品秘钥，直接进入下一步

![image-20211219174054021](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219174054021.png)

## 系统版本选择

Standard 标准版无图形界面

Standard（Desktop Experience）标准版有图形界面

DataCenter 数据中心版无图形界面

DataCenter（Desktop Experience）数据中心版有图形界面

本次安装选择Standard（Desktop Experience）

![image-20211219175249325](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219175249325.png)

## 许可协议

接受许可协议直接下一步

![image-20211219175529518](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219175529518.png)

## 安装模式

升级或自定义

本次安装为首次安装，选择自定义

![image-20211219180728358](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219180728358.png)

## 系统安装位置

本次安装只有一个磁盘，使用默认选择直接进入下一步

![image-20211219180931021](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219180931021.png)

## 账户设置

设置administrator账户的密码，系统安装完成

![image-20211219181512238](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20211219181512238.png)

## 系统激活

使用管理员身份运行cmd或powershell

输入以下命令修改KMS服务器

```shell
slmgr /skms kms.03k.org
```

输入以下命令设置系统秘钥（以下使用秘钥为windows server 2022 标准版秘钥，如果无法使用请自行百度搜索更换）

```shell
slmgr /ipk VDYBN-27WPP-V4HQT-9VMD4-VMK7H
```

输入以下命令开始激活系统

```shell
slmgr /ato
```

如果激活失败请尝试更换系统秘钥并重新激活，反复操作直到系统激活成功

# 手动安装网卡驱动

## 确认网卡型号

如果系统无法识别出网络适配器的型号，则需要借助一些第三方的工具来帮助我们识别网络适配器的型号

推荐使用软件AIDA64

![image-20220110165558609](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110165558609.png)

在AIDA64中可以看到当前设备的网卡型号，之后可以通过型号去下载对应的驱动

![image-20220110165705868](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110165705868.png)

## 安装驱动

在设备管理器中找到系统未能正确识别的网络设备，右键点击选择更新驱动程序

![image-20220110170144611](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170144611.png)

![image-20220110170215544](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170215544.png)

![image-20220110170258766](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170258766.png)

![image-20220110170345007](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170345007.png)

![image-20220110170441377](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170441377.png)

![image-20220110170616287](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220110170616287.png)

# AD域迁移流程

主域：cica.com

主服务器系统：windows server 2003 R2

主服务器IP地址：192.168.1.10

辅助服务器系统：windows server 2016 datacenter

辅助服务器IP地址：192.168.1.48

## 主服务器

### 关闭防火墙

![image-20220119153151233](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153151233.png)

### 提升林和域等级

![image-20220119153404269](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153404269.png)

![image-20220119153452044](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153452044.png)

![image-20220119153516603](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153516603.png)

![image-20220119153608287](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153608287.png)

### 开启服务

![image-20220119153656693](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153656693.png)

## 辅助服务器

### 修改DNS

![image-20220119153924710](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119153924710.png)

### 安装AD域

![image-20220119154058289](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119154058289.png)

![image-20220119154249083](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119154249083.png)

### 提升为域控制器

![image-20220119154449153](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119154449153.png)

![image-20220119155130874](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119155130874.png)

![image-20220119155221316](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220119155221316.png)

后面全默认设置即可，等待提升完成电脑自动重启

重启后使用域管理员账户登录，辅助域配置完成。

## 夺权

### 主服务器关机或断网

此处模拟主服务器宕机

### 检查辅助域是否正确工作

在主域关闭后，检查辅助域是否能够正常使用

打开Windows管理工具 -> AD域站点或服务

如果打开没有报错并如下图所示，则表示辅助域正常工作

![image-20220125113634833](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125113634833.png)+

### 修改DNS

![image-20220125120355502](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125120355502.png)

### 辅助域无法正常工作（正常跳过此步）

打开注册表编辑器

![image-20220125114734284](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125114734284.png)

注册表路径

```shell
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative Replica Sets\{GUID（非固定）}
```

修改注册表

![image-20220125114955609](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125114955609.png)

修改完成后使用控制台命令执行以下两段命令

```shell
Net stop netlogon & net start netlogon
```

```shell
Net stop ntfrs & net start ntfrs
```

执行完成后再次检查辅助域是否正确工作

### 抢夺5大角色

使用控制台命令执行以下命令

```shell
netdom query fsmo
```

执行结果可看到当前主域所在的服务器

如图：

![image-20220125115559541](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125115559541.png)

执行以下命令连接到域

```shell
ntdsutil
```

```shell
roles
```

```shell
connections
```

```shell
connect to server [域全名]
```

如图：

![image-20220125115818308](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125115818308.png)

依次执行以下5条命令抢夺角色

执行弹窗全选“是”

```shell
Seize infrastructure master
```

```shell
Seize naming master
```

```shell
Seize PDC
```

```shell
Seize RID master
```

```shell
Seize schema master
```

### 复制DC配置

Windows管理工具 -> AD域站点或服务

![image-20220125120510212](https://raw.githubusercontent.com/twyyy/tw97-document/master/images/image-20220125120510212.png)

### IP地址修改

修改辅助域的IP地址为主域的IP地址

至此完成了辅助域在主域宕机的情况下成功夺权。