<center><font size=7><b>openEuler内核热升级</b></font></center>

+++

[TOC]

+++

# 一、实验要求

## 1、<font color='red'>硬件要求</font>

- 目前**仅支持**<font color='red'><b>arm64</b></font>架构

## 2、环境要求

- 操作系统：`openEuler-22.03-LTS-everything-aarch64`

## 3、软件要求

<style>
    table td,table td {
        text-align:margin-left;
        vertical-align: middle;
    }
    table tr:odd{
        background:#faa;
    }
</style>
<table border="1">
    <tr>
        <td><center>所需材料</center></td>
        <td><center>下载链接</center></td>
    </tr>
    <tr>
        <td>openEuler-22.03-LTS-everything-aarch64-dvd.iso</td>
        <td>https://mirrors.tuna.tsinghua.edu.cn/openeuler/openEuler-22.03-LTS/ISO/aarch64/openEuler-22.03-LTS-everything-aarch64-dvd.iso</td>
    </tr>
    <tr>
        <td>kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm</td>
        <td>https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm</td>
    </tr>
    <tr>
        <td>kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm</td>
        <td>https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm</td>
    </tr>

## 4、参考资料

[安装与部署 (openeuler.org)](https://docs.openeuler.org/zh/docs/22.03_LTS/docs/KernelLiveUpgrade/安装与部署.html)

[openEuler内核热升级介绍(2022-10-11)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV17G41177mw/?spm_id_from=333.337.search-card.all.click&vd_source=c294cf5984fae71a727bed4ba7a4f367)

[终于有论坛了！内核热升级相关的内容可以在这里更新和提问了 - 互助交流区 - openEuler 论坛](https://forum.openeuler.org/t/topic/66/3)

[【openEuler 21.03】kernel rpm包安装失败 报错需要headers · Issue #I3AIZP · openEuler/kernel - Gitee.com](https://gitee.com/openeuler/kernel/issues/I3AIZP)

[update2101补丁版本内核升级失败 · Issue #I2O8M4 · src-openEuler/kernel - Gitee.com](https://gitee.com/src-openeuler/kernel/issues/I2O8M4)

# 二、实验准备

## 1、安装`openEuler-22.03-LTS`

​		查看以下内核版本

```bash
[root@openEuler2203 ~]# uname -r
5.10.0-60.18.0.50.oe2203.aarch64
[root@openEuler2203 ~]#
```



## 2、根据软件要求里的表格下载所需材料

```bash
[root@openEuler2203 ~]# wget https://mirrors.tuna.tsinghua.edu.cn/openeuler/openEuler-22.03-LTS/ISO/aarch64/openEuler-22.03-LTS-everything-aarch64-dvd.iso
--2023-01-17 10:53:43--  https://mirrors.tuna.tsinghua.edu.cn/openeuler/openEuler-22.03-LTS/ISO/aarch64/openEuler-22.03-LTS-everything-aarch64-dvd.iso

[root@openEuler2203 ~]# wget https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
--2023-01-17 10:53:52--  https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
正在解析主机 repo.openeuler.org (repo.openeuler.org)... 49.0.230.196
正在连接 repo.openeuler.org (repo.openeuler.org)|49.0.230.196|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：40952781 (39M) [application/x-redhat-package-manager]
正在保存至: “kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm”

kernel-5.10.0-106.18.0.68.oe2 100%[=================================================>]  39.05M   957KB/s  用时 15s

2023-01-17 10:54:07 (2.59 MB/s) - 已保存 “kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm” [40952781/40952781])

[root@openEuler2203 ~]# wget https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
--2023-01-17 10:54:48--  https://repo.openeuler.org/openEuler-22.09/everything/aarch64/Packages/kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
正在解析主机 repo.openeuler.org (repo.openeuler.org)... 49.0.230.196
正在连接 repo.openeuler.org (repo.openeuler.org)|49.0.230.196|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：14167393 (14M) [application/x-redhat-package-manager]
正在保存至: “kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm”

kernel-devel-5.10.0-106.18.0. 100%[=================================================>]  13.51M  1.17MB/s  用时 6.3s


2023-01-17 10:54:55 (2.15 MB/s) - 已保存 “kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm” [14167393/14167393])

[root@openEuler2203 ~]#
```

## 3、挂载iso文件并配置

### （1）、挂载openEuler的iso文件

```bash
[root@openEuler2203 ~]# mount openEuler-22.03-LTS-everything-aarch64-dvd.iso /mnt
mount: /mnt: WARNING: source write-protected, mounted read-only.
[root@openEuler2203 ~]#
```

### （2）配置本地yum源

```bash
[root@openEuler2203 ~]# vim /etc/yum.repos.d/local.repo
```

​		配置内容如下：

```ini
[local]
name=local
baseurl=file:///mnt
gpgcheck=1
enabled=1
```

### （3）将RPM数字签名的GPG公钥导入系统

```bash
[root@openEuler2203 ~]# rpm --import /mnt/RPM-GPG-KEY-openEuler
[root@openEuler2203 ~]#
```

## 4、确认安装的系统版本并且关闭selinux

​		运行以下命令：

```bash
[root@openEuler2203 ~]# cat /etc/os-release
NAME="openEuler"
VERSION="22.03 LTS"
ID="openEuler"
VERSION_ID="22.03"
PRETTY_NAME="openEuler 22.03 LTS"
ANSI_COLOR="0;31"

[root@openEuler2203 ~]#
```

### （1）关闭selinux

```bash
[root@openEuler2203 ~]# setenforce Permissive
[root@openEuler2203 ~]#
```

### （2）更新selinux配置

```bash
[root@openEuler2203 ~]# vim /etc/selinux/config
```



​		修改以下配置项

```ini
SELINUX=permissive
```

## 5、安装nvwa并确认版本

### （1）安装nvwa

```bash
[root@openEuler2203 ~]# dnf install nvwa -y
local                                                                                    94 MB/s |  16 MB     00:00
Last metadata expiration check: 0:00:05 ago on 2023年01月17日 星期二 11时03分24秒.
Dependencies resolved.
========================================================================================================================
 Package                     Architecture             Version                             Repository               Size
========================================================================================================================
Installing:
 nvwa                        aarch64                  0.2-3.oe2203                        update                  1.8 M
Installing dependencies:
 criu                        aarch64                  3.16.1-2.oe2203                     local                   480 k
 libnet                      aarch64                  1.2-1.oe2203                        local                    43 k
 protobuf-c                  aarch64                  1.4.0-2.oe2203                      update                  113 k

Transaction Summary
========================================================================================================================
Install  4 Packages

Total size: 2.4 M
Total download size: 1.9 M
Installed size: 9.1 M
Downloading Packages:
(1/2): protobuf-c-1.4.0-2.oe2203.aarch64.rpm                                            1.2 MB/s | 113 kB     00:00
(2/2): nvwa-0.2-3.oe2203.aarch64.rpm                                                    7.1 MB/s | 1.8 MB     00:00
------------------------------------------------------------------------------------------------------------------------
Total                                                                                   7.5 MB/s | 1.9 MB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                1/1
  Installing       : protobuf-c-1.4.0-2.oe2203.aarch64                                                              1/4
  Installing       : libnet-1.2-1.oe2203.aarch64                                                                    2/4
  Running scriptlet: libnet-1.2-1.oe2203.aarch64                                                                    2/4
  Installing       : criu-3.16.1-2.oe2203.aarch64                                                                   3/4
  Installing       : nvwa-0.2-3.oe2203.aarch64                                                                      4/4
  Running scriptlet: nvwa-0.2-3.oe2203.aarch64                                                                      4/4
  Verifying        : criu-3.16.1-2.oe2203.aarch64                                                                   1/4
  Verifying        : libnet-1.2-1.oe2203.aarch64                                                                    2/4
  Verifying        : nvwa-0.2-3.oe2203.aarch64                                                                      3/4
  Verifying        : protobuf-c-1.4.0-2.oe2203.aarch64                                                              4/4

Installed:
  criu-3.16.1-2.oe2203.aarch64 libnet-1.2-1.oe2203.aarch64 nvwa-0.2-3.oe2203.aarch64 protobuf-c-1.4.0-2.oe2203.aarch64

Complete!
[root@openEuler2203 ~]#
```

​		检查版本：

```bash
[root@openEuler2203 ~]# rpm -qi nvwa
Name        : nvwa
Version     : 0.2
Release     : 3.oe2203
Architecture: aarch64
Install Date: 2023年01月17日 星期二 11时03分34秒
Group       : Unspecified
Size        : 7325772
License     : MulanPSL-2.0 and Apache-2.0 and MIT and MPL-2.0
Signature   : RSA/SHA1, 2023年01月11日 星期三 15时07分50秒, Key ID d557065eb25e7f66
Source RPM  : nvwa-0.2-3.oe2203.src.rpm
Build Date  : 2023年01月11日 星期三 15时08分30秒
Build Host  : obs-worker-backend-test-arm-0005
Packager    : http://openeuler.org
Vendor      : http://openeuler.org
URL         : https://gitee.com/openeuler/nvwa
Summary     : a tool used for openEuler kernel update
Description :
A tool used to automate the process of seamless update of the openEuler.
[root@openEuler2203 ~]#
```



### （2）安装其他相关工具

```bash
[root@openEuler2203 ~]# dnf install criu kexec-tools -y
Last metadata expiration check: 0:01:09 ago on 2023年01月17日 星期二 11时03分36秒.
Package criu-3.16.1-2.oe2203.aarch64 is already installed.
Package kexec-tools-2.0.23-4.oe2203.aarch64 is already installed.
Dependencies resolved.
========================================================================================================================
 Package                      Architecture             Version                            Repository               Size
========================================================================================================================
Upgrading:
 kexec-tools                  aarch64                  2.0.23-8.oe2203                    update                  339 k

Transaction Summary
========================================================================================================================
Upgrade  1 Package

Total download size: 339 k
Downloading Packages:
kexec-tools-2.0.23-8.oe2203.aarch64.rpm                                                 2.9 MB/s | 339 kB     00:00
------------------------------------------------------------------------------------------------------------------------
Total                                                                                   2.9 MB/s | 339 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                1/1
  Running scriptlet: kexec-tools-2.0.23-8.oe2203.aarch64                                                            1/1
  Upgrading        : kexec-tools-2.0.23-8.oe2203.aarch64                                                            1/2
  Running scriptlet: kexec-tools-2.0.23-8.oe2203.aarch64                                                            1/2
  Running scriptlet: kexec-tools-2.0.23-4.oe2203.aarch64                                                            2/2
  Cleanup          : kexec-tools-2.0.23-4.oe2203.aarch64                                                            2/2
  Running scriptlet: kexec-tools-2.0.23-4.oe2203.aarch64                                                            2/2
  Verifying        : kexec-tools-2.0.23-8.oe2203.aarch64                                                            1/2
  Verifying        : kexec-tools-2.0.23-4.oe2203.aarch64                                                            2/2

Upgraded:
  kexec-tools-2.0.23-8.oe2203.aarch64

Complete!
[root@openEuler2203 ~]#
```



### （3）运行nvwa

```bash
[root@openEuler2203 ~]# systemctl enable nvwa
Created symlink /etc/systemd/system/nvwa.service → /usr/lib/systemd/system/nvwa.service.
Created symlink /etc/systemd/system/multi-user.target.wants/nvwa.service → /usr/lib/systemd/system/nvwa.service.
[root@openEuler2203 ~]# systemctl start nvwa
[root@openEuler2203 ~]# systemctl status nvwa
● nvwa.service - NVWA server
     Loaded: loaded (/usr/lib/systemd/system/nvwa.service; enabled; vendor preset: disabled)
     Active: active (running) since Tue 2023-01-17 11:06:11 CST; 11s ago
   Main PID: 5687 (nvwa)
      Tasks: 7 (limit: 5552)
     Memory: 10.5M
     CGroup: /system.slice/nvwa.service
             └─5687 /usr/bin/nvwa -server 1

1月 17 11:06:11 openEuler2203 systemd[1]: Starting NVWA server...
1月 17 11:06:11 openEuler2203 systemd[1]: Started NVWA server.
[root@openEuler2203 ~]#
```



## 6、安装内核RPM包

​		刚刚下载了两个内核RPM包

```bash
[root@openEuler2203 ~]# ll | grep "kernel"
-rw-r--r--. 1 root root  40M  9月 30 09:12 kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
-rw-r--r--. 1 root root  14M  9月 30 09:06 kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
[root@openEuler2203 ~]#
```

​		首先安装`kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm`

```bash
[root@openEuler2203 ~]# rpm -ivh kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
Verifying...                          ################################# [100%]
准备中...                          ################################# [100%]
正在升级/安装...
   1:kernel-5.10.0-106.18.0.68.oe2209 ################################# [100%]
Error! echo
Your kernel headers for kernel 5.10.0-106.18.0.68.oe2209.aarch64 cannot be found at
/lib/modules/5.10.0-106.18.0.68.oe2209.aarch64/build or /lib/modules/5.10.0-106.18.0.68.oe2209.aarch64/source.
Error! echo
Your kernel headers for kernel 5.10.0-106.18.0.68.oe2209.aarch64 cannot be found at
/lib/modules/5.10.0-106.18.0.68.oe2209.aarch64/build or /lib/modules/5.10.0-106.18.0.68.oe2209.aarch64/source.
[root@openEuler2203 ~]#
```

​		根据[update2101补丁版本内核升级失败 · Issue #I2O8M4 · src-openEuler/kernel - Gitee.com](https://gitee.com/src-openeuler/kernel/issues/I2O8M4)文章的说明环境上安装了dkms，并且用dkms编译了kmod-kvdo。然后在升级kernel的时候,不同时升级kernel-devel包才会复现。

​		所以我才说要下载`kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm`这个包

```bash
[root@openEuler2203 ~]# rpm -ivh kernel-devel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
Verifying...                          ################################# [100%]
准备中...                          ################################# [100%]
正在升级/安装...
   1:kernel-devel-5.10.0-106.18.0.68.o################################# [100%]
[root@openEuler2203 ~]#
```



​		然后再安装`kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm`就不会报错了。

```bash
[root@openEuler2203 ~]# rpm -ivh kernel-5.10.0-106.18.0.68.oe2209.aarch64.rpm
Verifying...                          ################################# [100%]
准备中...                          ################################# [100%]
        软件包 kernel-5.10.0-106.18.0.68.oe2209.aarch64 已经安装
[root@openEuler2203 ~]#
```



​		然后查看一下`/boot`目录：

```bash
[root@openEuler2203 ~]# ll /boot
总用量 108M
-rw-r--r--. 1 root root 180K  9月 24 08:00 config-5.10.0-106.18.0.68.oe2209.aarch64
-rw-r--r--. 1 root root 179K  3月 27  2022 config-5.10.0-60.18.0.50.oe2203.aarch64
drwxr-xr-x. 2 root root 4.0K  2月 16  2022 dracut
drwxr-xr-x. 2 root root 4.0K  1月 17 11:10 dtb-5.10.0-106.18.0.68.oe2209.aarch64
drwxr-xr-x. 2 root root 4.0K  1月 15 22:18 dtb-5.10.0-60.18.0.50.oe2203.aarch64
drwx------. 3 root root 4.0K  1月  1  1970 efi
drwx------. 2 root root 4.0K  1月 15 22:20 grub2
-rw-------. 1 root root  24M  1月 17 11:11 initramfs-5.10.0-106.18.0.68.oe2209.aarch64.img
-rw-------. 1 root root  25M  1月 15 22:20 initramfs-5.10.0-60.18.0.50.oe2203.aarch64.img
-rw-------. 1 root root  30M  1月 15 22:21 initramfs-5.10.0-60.18.0.50.oe2203.aarch64kdump.img
drwxr-xr-x. 3 root root 4.0K  1月 15 22:18 loader
drwx------. 2 root root  16K  1月 15 22:13 lost+found
-rw-r--r--. 1 root root 283K  9月 24 08:00 symvers-5.10.0-106.18.0.68.oe2209.aarch64.gz
-rw-r--r--. 1 root root 282K  3月 27  2022 symvers-5.10.0-60.18.0.50.oe2203.aarch64.gz
-rw-r--r--. 1 root root 4.8M  9月 24 08:00 System.map-5.10.0-106.18.0.68.oe2209.aarch64
-rw-r--r--. 1 root root 4.7M  3月 27  2022 System.map-5.10.0-60.18.0.50.oe2203.aarch64
-rwxr-xr-x. 1 root root  11M  9月 24 08:00 vmlinuz-5.10.0-106.18.0.68.oe2209.aarch64
-rwxr-xr-x. 1 root root  10M  3月 27  2022 vmlinuz-5.10.0-60.18.0.50.oe2203.aarch64
[root@openEuler2203 ~]#
```



## 三、用`nvwa`进行热升级

​		我们先来看看`nvwa`的用法：

```bash
[root@openEuler2203 ~]# nvwa help
NAME:
   nvwa - a tool used for openEuler kernel update.     # 用于openEuler内核更新的工具

USAGE:
   nvwa [global options] command [command options] [arguments...]

COMMANDS:
   update   specify kernel version for nvwa to update
   init     init nvwa running environment
   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h  show help (default: false)
[root@openEuler2203 ~]#

翻译如下：

姓名：
	nvwa-用于openEuler内核更新的工具。
用法：
	nvwa[全局选项]命令[命令选项][参数…]
命令：
	update   指定要更新的nvwa内核版本
	init     init nvwa运行环境
	help，h  显示命令列表或一个命令的帮助
全局选项：
	--help，-h显示帮助（默认值：false）
```

​		然后用`update`更新：

```bash
[root@openEuler2203 ~]# nvwa update 5.10.0-106.18.0.68.oe2209.aarch64
```

​		然后系统会重启，然后对比以下前后版本：

```bash
[root@openEuler2203 ~]# uname -r
5.10.0-60.18.0.50.oe2203.aarch64
[root@openEuler2203 ~]# uname -r
5.10.0-106.18.0.68.oe2209.aarch64
```

