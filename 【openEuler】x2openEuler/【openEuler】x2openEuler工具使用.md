<center><font size=7><b>x2openEuler工具使用</b></font></center>

+++

[TOC]

+++

# 一、关于`x2openEuler`

​		x2openEuler工具是一款将源操作系统迁移到目标操作系统的迁移工具套件，具有批量化原地升级能力，当前支持将源OS升级至openEuler 20.03。为解决客户升级操作系统过程中人工投入大、准确率低、无法批量化处理导致整体效率低下的痛点，x2openEuler工具提供简单易用的操作界面，您可以批量添加待升级节点进行迁移分析，设计迁移方案并对兼容性问题进行迁移适配，最后对已适配的待升级节点批量升级，实现端到端的无感迁移。

# 二、工具准备

| 工具                                | 下载连接                                                     |
| :---------------------------------- | ------------------------------------------------------------ |
| x2openEuler-core-2.0.0-4.x86_64.rpm | https://repo.oepkgs.net/openEuler/rpm/openEuler-20.03-LTS-SP1/contrib/x2openEuler/x86_64/Packages/x2openEuler-core-2.0.0-4.x86_64.rpm |
| CentOS7.6.1810                      | https://vault.centos.org/7.6.1810/isos/x86_64/CentOS-7-x86_64-Everything-1810.iso |

# 三、前期准备

## 1、安装CentOS7

​		安装好CentOS7后克隆两份。

![image-20230119133951846](.\image\image-20230119133951846.png)

| CentOS-7.6Evetything1 | CentOS-7.6Evetything2 |
| --------------------- | --------------------- |
| 192.168.74.162        | 192.168.74.164        |

## 2、下载`x2openEuler`

​		在`CentOS-7.6Evetything1`上下载`x2openEuler`。

```bash
[root@CentOS76 ~]# wget https://repo.oepkgs.net/openEuler/rpm/openEuler-20.03-LTS-SP1/contrib/x2openEuler/x86_64/Packages/x2openEuler-core-2.0.0-4.x86_64.rpm
--2023-01-19 13:27:46--  https://repo.oepkgs.net/openEuler/rpm/openEuler-20.03-LTS-SP1/contrib/x2openEuler/x86_64/Packages/x2openEuler-core-2.0.0-4.x86_64.rpm
正在解析主机 repo.oepkgs.net (repo.oepkgs.net)... 124.70.29.98
正在连接 repo.oepkgs.net (repo.oepkgs.net)|124.70.29.98|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：384066936 (366M) [application/x-redhat-package-manager]
正在保存至: “x2openEuler-core-2.0.0-4.x86_64.rpm”

100%[==============================================================================>] 384,066,936 3.88MB/s 用时 2m 26s

2023-01-19 13:30:14 (2.50 MB/s) - 已保存 “x2openEuler-core-2.0.0-4.x86_64.rpm” [384066936/384066936])

[root@CentOS76 ~]#
```

## 3、安装`x2openEuler`

```bash
[root@CentOS76 ~]# yum install -y x2openEuler-core-2.0.0-4.x86_64.rpm
已加载插件：fastestmirror, langpacks
正在检查 x2openEuler-core-2.0.0-4.x86_64.rpm: x2openEuler-core-2.0.0-4.x86_64
x2openEuler-core-2.0.0-4.x86_64.rpm 将被安装
正在解决依赖关系
--> 正在检查事务
---> 软件包 x2openEuler-core.x86_64.0.2.0.0-4 将被 安装
--> 正在处理依赖关系 java-1.8.0-openjdk-devel，它被软件包 x2openEuler-core-2.0.0-4.x86_64 需要
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
--> 正在处理依赖关系 expect，它被软件包 x2openEuler-core-2.0.0-4.x86_64 需要
--> 正在检查事务
---> 软件包 expect.x86_64.0.5.45-14.el7_1 将被 安装
--> 正在处理依赖关系 libtcl8.5.so()(64bit)，它被软件包 expect-5.45-14.el7_1.x86_64 需要
---> 软件包 java-1.8.0-openjdk-devel.x86_64.1.1.8.0.352.b08-2.el7_9 将被 安装
--> 正在处理依赖关系 java-1.8.0-openjdk(x86-64) = 1:1.8.0.352.b08-2.el7_9，它被软件包 1:java-1.8.0-openjdk-devel-1.8.0.352.b08-2.el7_9.x86_64 需要
--> 正在检查事务
---> 软件包 java-1.8.0-openjdk.x86_64.1.1.8.0.181-7.b13.el7 将被 升级
---> 软件包 java-1.8.0-openjdk.x86_64.1.1.8.0.352.b08-2.el7_9 将被 更新
--> 正在处理依赖关系 java-1.8.0-openjdk-headless(x86-64) = 1:1.8.0.352.b08-2.el7_9，它被软件包 1:java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64 需要
---> 软件包 tcl.x86_64.1.8.5.13-8.el7 将被 安装
--> 正在检查事务
---> 软件包 java-1.8.0-openjdk-headless.x86_64.1.1.8.0.181-7.b13.el7 将被 升级
---> 软件包 java-1.8.0-openjdk-headless.x86_64.1.1.8.0.352.b08-2.el7_9 将被 更新
--> 正在处理依赖关系 tzdata-java >= 2022d，它被软件包 1:java-1.8.0-openjdk-headless-1.8.0.352.b08-2.el7_9.x86_64 需要
--> 正在检查事务
---> 软件包 tzdata-java.noarch.0.2018e-3.el7 将被 升级
---> 软件包 tzdata-java.noarch.0.2022g-1.el7 将被 更新
--> 解决依赖关系完成

依赖关系解决

========================================================================================================================
 Package                          架构        版本                          源                                     大小
========================================================================================================================
正在安装:
 x2openEuler-core                 x86_64      2.0.0-4                       /x2openEuler-core-2.0.0-4.x86_64      1.9 G
为依赖而安装:
 expect                           x86_64      5.45-14.el7_1                 base                                  262 k
 java-1.8.0-openjdk-devel         x86_64      1:1.8.0.352.b08-2.el7_9       updates                               9.8 M
 tcl                              x86_64      1:8.5.13-8.el7                base                                  1.9 M
为依赖而更新:
 java-1.8.0-openjdk               x86_64      1:1.8.0.352.b08-2.el7_9       updates                               316 k
 java-1.8.0-openjdk-headless      x86_64      1:1.8.0.352.b08-2.el7_9       updates                                33 M
 tzdata-java                      noarch      2022g-1.el7                   updates                               185 k

事务概要
========================================================================================================================
安装  1 软件包 (+3 依赖软件包)
升级           ( 3 依赖软件包)

总计：2.0 G
总下载量：46 M
Downloading packages:
No Presto metadata available for updates
(1/6): expect-5.45-14.el7_1.x86_64.rpm                                                           | 262 kB  00:00:00
(2/6): java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64.rpm                                       | 316 kB  00:00:00
(3/6): tcl-8.5.13-8.el7.x86_64.rpm                                                               | 1.9 MB  00:00:01
(4/6): java-1.8.0-openjdk-devel-1.8.0.352.b08-2.el7_9.x86_64.rpm                                 | 9.8 MB  00:00:02
(5/6): tzdata-java-2022g-1.el7.noarch.rpm                                                        | 185 kB  00:00:00
(6/6): java-1.8.0-openjdk-headless-1.8.0.352.b08-2.el7_9.x86_64.rpm                              |  33 MB  00:00:06
------------------------------------------------------------------------------------------------------------------------
总计                                                                                    6.4 MB/s |  46 MB  00:00:07
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在更新    : tzdata-java-2022g-1.el7.noarch                                                                     1/10
  正在更新    : 1:java-1.8.0-openjdk-headless-1.8.0.352.b08-2.el7_9.x86_64                                         2/10
warning: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/net.properties created as /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/net.properties.rpmnew
warning: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/blacklisted.certs created as /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/blacklisted.certs.rpmnew
warning: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.policy created as /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.policy.rpmnew
warning: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.security created as /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.security.rpmnew
restored /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/blacklisted.certs.rpmnew to /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/blacklisted.certs
restored /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.policy.rpmnew to /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.policy
restored /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.security.rpmnew to /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/security/java.security
restored /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/net.properties.rpmnew to /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/lib/net.properties
  正在更新    : 1:java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64                                                  3/10
  正在安装    : 1:java-1.8.0-openjdk-devel-1.8.0.352.b08-2.el7_9.x86_64                                            4/10
  正在安装    : 1:tcl-8.5.13-8.el7.x86_64                                                                          5/10
  正在安装    : expect-5.45-14.el7_1.x86_64                                                                        6/10
  正在安装    : x2openEuler-core-2.0.0-4.x86_64                                                                    7/10
Please enter /usr/local/x2openEuler/portal/service/ and execute bash service_start.sh to start service.
  清理        : 1:java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64                                                    8/10
  清理        : 1:java-1.8.0-openjdk-headless-1.8.0.181-7.b13.el7.x86_64                                           9/10
  清理        : tzdata-java-2018e-3.el7.noarch                                                                    10/10
  验证中      : x2openEuler-core-2.0.0-4.x86_64                                                                    1/10
  验证中      : 1:tcl-8.5.13-8.el7.x86_64                                                                          2/10
  验证中      : 1:java-1.8.0-openjdk-headless-1.8.0.352.b08-2.el7_9.x86_64                                         3/10
  验证中      : expect-5.45-14.el7_1.x86_64                                                                        4/10
  验证中      : tzdata-java-2022g-1.el7.noarch                                                                     5/10
  验证中      : 1:java-1.8.0-openjdk-devel-1.8.0.352.b08-2.el7_9.x86_64                                            6/10
  验证中      : 1:java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64                                                  7/10
  验证中      : 1:java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64                                                    8/10
  验证中      : tzdata-java-2018e-3.el7.noarch                                                                     9/10
  验证中      : 1:java-1.8.0-openjdk-headless-1.8.0.181-7.b13.el7.x86_64                                          10/10

已安装:
  x2openEuler-core.x86_64 0:2.0.0-4

作为依赖被安装:
  expect.x86_64 0:5.45-14.el7_1   java-1.8.0-openjdk-devel.x86_64 1:1.8.0.352.b08-2.el7_9   tcl.x86_64 1:8.5.13-8.el7

作为依赖被升级:
  java-1.8.0-openjdk.x86_64 1:1.8.0.352.b08-2.el7_9      java-1.8.0-openjdk-headless.x86_64 1:1.8.0.352.b08-2.el7_9
  tzdata-java.noarch 0:2022g-1.el7

完毕！
[root@CentOS76 ~]#
```

## 4、执行bash

```bash
[root@CentOS76 ~]# cd /usr/local/x2openEuler/portal/service/
[root@CentOS76 service]# ll
总用量 68
-rwxr-x---. 1 root root 14648 12月 28 00:00 change_ip_x2openEuler.sh
-rwxr-x---. 1 root root    57 12月 28 00:00 const.conf
-rwxr-x---. 1 root root  1839 12月 28 00:00 delete_file.sh
-rwxr-x---. 1 root root  3307 12月 28 00:00 gunicorn_x2openEuler
-rwxr-x---. 1 root root   579 12月 28 00:00 gunicorn_x2openEuler.service
-rwxr-x---. 1 root root   719 12月 28 00:00 nginx_x2openEuler
-rwxr-x---. 1 root root   591 12月 28 00:00 nginx_x2openEuler.service
-rwxr-x---. 1 root root   762 12月 28 00:00 service_daemon.sh
-rwxr-x---. 1 root root   231 12月 28 00:00 service_gunicorn.sh
-rwxr-x---. 1 root root  3179 12月 28 00:00 service_nginx.sh
-rwxr-x---. 1 root root 15234 12月 28 00:00 service_start.sh
```



```bash
[root@CentOS76 service]# bash service_start.sh
Start Nginx service and Gunicorn service
Ip address list:
sequence_number         ip_address              device
[1]                     192.168.74.162          ens33
[2]                     192.168.122.1           virbr0
Enter the sequence number of listed ip as web server ip(default: 1):
Set the web server IP address 192.168.74.162
Please enter HTTPS port(default: 18082):
The HTTPS port 18082 is valid.  Set the HTTPS port to 18082 (y/n default: y):
Set the HTTPS port 18082
Please enter gunicorn port(default: 18080):
The GUNICORN port 18080 is valid.  Set the GUNICORN port to 18080 (y/n default: y):
Set the GUNICORN port 18080
To ensure successful running of the tool, enable the web service port and reload the configuration as follows:
   1.Enable the web service port: firewall-cmd --add-port=18082/tcp --permanent
   2.Reload the configuration: firewall-cmd --reload
   3.Check whether the port is enabled: firewall-cmd --query-port=18082/tcp
Are you agree to run the above command to enable the port?(y/n,default:y)
Port 18082 is enabled successfully.
The Nginx and Gunicorn ports are set up successfully.
Installing the django dependent environment.
The django dependency environment is installed successfully.
Generating the Django secret key.
Generate the Django secret key successfully.
Migrations for 'certificatemanager':
  /usr/local/x2openEuler/portal/src/certificatemanager/migrations/0001_initial.py
    - Create model CertificateInfo
    - Create model CertPathConfig
    - Create model ScheduleTask
Migrations for 'config':
  /usr/local/x2openEuler/portal/src/config/migrations/0001_initial.py
    - Create model UserConfig
Migrations for 'operationlogmanager':
  /usr/local/x2openEuler/portal/src/operationlogmanager/migrations/0001_initial.py
    - Create model OperationLog
Migrations for 'taskmanager':
  /usr/local/x2openEuler/portal/src/taskmanager/migrations/0001_initial.py
    - Create model Node
    - Create model Repo
    - Create model Report
    - Create model Step
    - Create model Task
Migrations for 'usermanager':
  /usr/local/x2openEuler/portal/src/usermanager/migrations/0001_initial.py
    - Create model User
    - Create model FailedLogin
    - Create model LockedIp
    - Create model UserExtend
Migrations for 'weakpasswordmanager':
  /usr/local/x2openEuler/portal/src/weakpasswordmanager/migrations/0001_initial.py
    - Create model WeakPassword
Operations to perform:
  Apply all migrations: auth, certificatemanager, config, contenttypes, operationlogmanager, sessions, taskmanager, usermanager, weakpasswordmanager
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying certificatemanager.0001_initial... OK
  Applying config.0001_initial... OK
  Applying operationlogmanager.0001_initial... OK
  Applying sessions.0001_initial... OK
  Applying taskmanager.0001_initial... OK
  Applying usermanager.0001_initial... OK
  Applying weakpasswordmanager.0001_initial... OK
Installed 1 object(s) from 1 fixture(s)
Installed 1 object(s) from 1 fixture(s)
Installed 8 object(s) from 1 fixture(s)
Installed 52 object(s) from 1 fixture(s)
Installed 2 object(s) from 1 fixture(s)
Encrypting phase successfully.
It may take a few minutes to generate the certificate, please wait...
Certificate generated successfully. You can import the root certificate to the browser to mask security alarms when you access the tool. The root certificate is stored in /usr/local/x2openEuler/portal/thirdapp/nginx-install/webui/ca.crt.
Web console is now running, go to: https://192.168.74.162:18082/x2openEuler/#/login
[root@CentOS76 service]#
```

​		执行完bash后会有一个`https://192.168.74.162:18082/x2openEuler/#/login`，可以在浏览器中访问。

​		然后关闭防火墙

```bash
[root@CentOS76 service]# service firewalld stop
Redirecting to /bin/systemctl stop firewalld.service
[root@CentOS76 service]# setenforce 0
[root@CentOS76 service]#
```



## 5、访问上述网站

​		访问后效果如下：

![image-20230119135640636](.\image\image-20230119135640636.png)

​		第一次登录是需要设置密码的。设置完密码后登录进来。

![image-20230119135946496](.\image\image-20230119135946496.png)

## 6、安装`x2openEuler-client`

### （1）在`CentOS-7.6Evetything1`上找到`x2openEuler-client`

```bash
[root@CentOS76 service]# cd /etc/x2openEuler
[root@CentOS76 x2openEuler]# ll
总用量 36
dr-xr-x---. 5 x2openEuler x2openEuler   112 1月  19 13:45 config
dr-xr-x---. 9 x2openEuler x2openEuler   207 1月  19 13:46 database_2.0.0.630
-r--r-----. 1 x2openEuler x2openEuler 34828 12月 28 16:10 x2openEuler-client-2.0.0-2.noarch.rpm
[root@CentOS76 x2openEuler]#
```

### （2）把`x2openEuler-client`拷贝到`CentOS-7.6Evetything2`上并安装

```bash
[root@CentOS76 桌面]# yum install -y x2openEuler-client-2.0.0-2.noarch.rpm
已加载插件：fastestmirror, langpacks
正在检查 x2openEuler-client-2.0.0-2.noarch.rpm: x2openEuler-client-2.0.0-2.noarch
x2openEuler-client-2.0.0-2.noarch.rpm 将被安装
正在解决依赖关系
--> 正在检查事务
---> 软件包 x2openEuler-client.noarch.0.2.0.0-2 将被 安装
--> 解决依赖关系完成

依赖关系解决

========================================================================================================================
 Package                       架构              版本               源                                             大小
========================================================================================================================
正在安装:
 x2openEuler-client            noarch            2.0.0-2            /x2openEuler-client-2.0.0-2.noarch            124 k

事务概要
========================================================================================================================
安装  1 软件包

总计：124 k
安装大小：124 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : x2openEuler-client-2.0.0-2.noarch                                                                   1/1
  验证中      : x2openEuler-client-2.0.0-2.noarch                                                                   1/1

已安装:
  x2openEuler-client.noarch 0:2.0.0-2

完毕！
[root@CentOS76 桌面]#
```

# 四、使用`x2openEuler`将`CentOS`升级到`openEuler`

## 1、在`CentOS-7.6Evetything2`上查看系统等信息

```bash
[root@CentOS76 ~]# uname -a
Linux CentOS76 3.10.0-957.el7.x86_64 #1 SMP Thu Nov 8 23:39:32 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
[root@CentOS76 ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

[root@CentOS76 ~]#
```

## 2、系统升级前准备

### （1）`新建任务`→`系统升级`

![image-20230119141910349](.\image\image-20230119141910349.png)

### （2）添加节点

​		任务名称随意，点击`添加节点`

![image-20230119142006181](.\image\image-20230119142006181.png)

​		配置框内容如下：

<style>
    table td,table td {
        text-align:left;
        vertical-align: middle;
    }
    table tr:odd{
        background:#faa;
    }
</style>
<table border="1">
    <tr>
        <td>待升级节点IP</td>
        <td>192.168.74.164</td>
    </tr>
    <tr>
        <td>节点别名</td>
        <td>（你喜欢就好）</td>
    </tr>
    <tr>
        <td>端口</td>
        <td>22</td>
    </tr>
    <tr>
        <td>用户名</td>
        <td>root</td>
    </tr>
    <tr>
        <td>认证方式</td>
        <td>密码认证</td>
    </tr>
    <tr>
        <td>密码</td>
        <td>（就是你要升级的系统的root密码）</td>
    </tr>
    <tr>
        <td>目标操作系统版本</td>
        <td>（看着选就行）</td>
    </tr>
    <tr>
        <td>业务软件(rpm包)</td>
        <td>x2openEuler-client-2.0.0-2.noarch.rpm</td>
    </tr>
</table>

​		repo源名称：（如果是ARM框架的就选`aarch`）

![image-20230119142729640](.\image\image-20230119142729640.png)

​		确认后点击`确定`：

![image-20230119142848112](.\image\image-20230119142848112.png)

​		然后看到提示信息：

![image-20230119142912807](.\image\image-20230119142912807.png)



![image-20230119143004544](.\image\image-20230119143004544.png)

## 3、升级系统

​		首先会进行连通性测试

![image-20230119143106726](.\image\image-20230119143106726.png)

​		大概需要一两分钟，测试通过后进行升级前检查。

![image-20230119143210222](.\image\image-20230119143210222.png)

​		升级前检查根据实际情况，时间长短不已，我这边大概用了5分钟。

![image-20230119143836047](.\image\image-20230119143836047.png)

​		检查完成后可以直接开始升级

![image-20230119143907350](.\image\image-20230119143907350.png)

​		升级大概用了半个小时？反正升级的时候会断网

![image-20230119151335136](.\image\image-20230119151335136.png)

```bash
[root@CentOS76 ~]# uname -a
Linux CentOS76 3.10.0-957.el7.x86_64 #1 SMP Thu Nov 8 23:39:32 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
[root@CentOS76 ~]# cat /etc/os-release
NAME="openEuler"
VERSION="20.03 (LTS-SP1)"
ID="openEuler"
VERSION_ID="20.03"
PRETTY_NAME="openEuler 20.03 (LTS-SP1)"
ANSI_COLOR="0;31"

[root@CentOS76 ~]#
```

​		这个时候看`os-release`就已经成了`openEuler`。但是内核还是原来的，需要一会重启。

![image-20230119151554788](.\image\image-20230119151554788.png)

​		这时候点击重启节点即可。

​		重启以后是这样的：

![image-20230119151821454](.\image\image-20230119151821454.png)

​		直接在Terminal操作即可，因为前面也说了openEuler是没有图形化界面的。

```bash
[root@CentOS76 ~]# uname -a
Linux CentOS76 4.19.90-2301.3.0.0184.oe1.x86_64 #1 SMP Wed Jan 11 12:11:56 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
[root@CentOS76 ~]# cat /etc/os-release
NAME="openEuler"
VERSION="20.03 (LTS-SP1)"
ID="openEuler"
VERSION_ID="20.03"
PRETTY_NAME="openEuler 20.03 (LTS-SP1)"
ANSI_COLOR="0;31"

[root@CentOS76 ~]#
```

​		这个时候看内核已经是更正了的。

​		另外等待系统稳定后就可以开始清理了。

![image-20230119152005982](.\image\image-20230119152005982.png)



![image-20230119152042974](.\image\image-20230119152042974.png)