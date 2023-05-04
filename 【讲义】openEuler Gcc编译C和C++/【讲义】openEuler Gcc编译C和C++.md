<center><font size=7><b>openEuler Gcc编译C/C++</b></font></center>

+++

[toc]

+++

- 实验环境：`openEuler-22.03-LTS-everything`

+++



# 一、课程介绍

​		关于openEuler的GCC编译方面，我们主要介绍一下几个内容：

​		1、GCC基本用法，包括安装、参数还有整个编译过程等都会做详细的介绍；因为我们知道GCC是目前跨平台的、几乎是不二之选，基本上我们都要用到GCC，不管你是在做嵌入式、还有安卓以及ios上面的应用都会涉及到GCC，像我们做一些视频应用，最终给安卓上面用的，我们还是通过GCC编译出相关库然后再去应用，也就是说GCC基本上以后是一定会碰到的，不管你是做什么样的项目。

​		2、GCC编译多文件。一个文件的编译很简单，多个文件编译就涉及到头文件、多个CPP中间的问题怎么结局

​		3、GCC编译动态库。我们知道，我们做一个项目，特别是这种跨区平台的项目，都可能代码就是一套的，你可能会用第三方的东西，用第三方的东西不可能每一次把源码全部编译一下，比如说你需不需要引用动态链接库，包括你自己做的部分功能你也需要把它编译成动态链接库，比如说我们需要明白怎么编译成动态链接库，你在解决第三方动态链接库的时候，你才能知道这个问题出在哪里，包括怎么引用、怎么运行，因为它的运行跟Windows上还是不太一样的，Linux上面直接运行就可以了



# 二、GCC介绍

## 1、gcc简介

- 名称：

​		— GUN Compiler Collection

- 管理与维护

​		—GNU项目

- 对C/C++编译的控制

​		— 预处理（Preprocessing）

​		— 编译（Compilation）

​		— 汇编（Assembly）

​		— 链接（Linking）

<blockquote>
GCC是什么，就是一个GUN Compiler Collection，它原来就是单独编译C语言的一个工具，现在已经编程了一个编译集了，它是包含了C、C++、Java等各种语言的编译，但我们目前给大家讲解一下C和C++的编译，它的管理维护是GNU项目来处理的。GCC这个工具经历了编译哪几个部分呢？预处理、编译、汇编和链接这四部分工作都是由它来做的，这个对应的如果是用Windows开发的，你会看到在Windows中的编译过程是被隐藏了，在GCC当中，我们是可以分别做处理的，但在Windows当中你肯定也会去碰到这些问题，你也回去做预处理、编译、汇编、链接，但汇编到链接一般只有相对于硬件开发的才有涉及到，预处理、编译和链接你是一定会碰到的，为什么？你知道在不同阶段会出现不同的错误。比如预处理会出现什么错误？宏定义错误，编译会出现什么错误？语法错误，这类的比方说一些函数没有返回，有待返回值的函数没有返回它也会提示，那你就知道编译阶段的错误就是代码错误，链接会是什么错误？找不到库文件、找不到资源。GCC它会涉及到整个编译流程后面的课程会把每一个步骤独立开来进行演示。下面我们把环境介绍一下。。。。
</blockquote>

## 2、准备

​		安装gcc

```bash
[root@openEuler2203 ~]# yum install -y gcc
Last metadata expiration check: 0:16:10 ago on 2023年02月20日 星期一 10时50分56秒.
Package gcc-10.3.1-10.oe2203.x86_64 is already installed.
Dependencies resolved.
========================================================================================================================
 Package                           Architecture           Version                          Repository              Size
========================================================================================================================
Upgrading:
 cpp                               x86_64                 10.3.1-11.oe2203                 update                 8.9 M
 gcc                               x86_64                 10.3.1-11.oe2203                 update                  29 M
 gcc-c++                           x86_64                 10.3.1-11.oe2203                 update                  11 M
 gcc-gdb-plugin                    x86_64                 10.3.1-11.oe2203                 update                 101 k
 gcc-gfortran                      x86_64                 10.3.1-11.oe2203                 update                  10 M
 libgcc                            x86_64                 10.3.1-11.oe2203                 update                  74 k
 libgfortran                       x86_64                 10.3.1-11.oe2203                 update                 785 k
 libgomp                           x86_64                 10.3.1-11.oe2203                 update                 228 k
 libquadmath                       x86_64                 10.3.1-11.oe2203                 update                 155 k
 libquadmath-devel                 x86_64                 10.3.1-11.oe2203                 update                  22 k
 libstdc++                         x86_64                 10.3.1-11.oe2203                 update                 529 k
 libstdc++-devel                   x86_64                 10.3.1-11.oe2203                 update                 2.2 M

Transaction Summary
========================================================================================================================
Upgrade  12 Packages

Total download size: 62 M
Downloading Packages:
(1/12): cpp-10.3.1-11.oe2203.x86_64.rpm                                                 151 kB/s | 8.9 MB     01:00
(2/12): gcc-gdb-plugin-10.3.1-11.oe2203.x86_64.rpm                                       87 kB/s | 101 kB     00:01
(3/12): gcc-c++-10.3.1-11.oe2203.x86_64.rpm                                             108 kB/s |  11 MB     01:40
(4/12): libgcc-10.3.1-11.oe2203.x86_64.rpm                                               77 kB/s |  74 kB     00:00
(5/12): libgfortran-10.3.1-11.oe2203.x86_64.rpm                                         210 kB/s | 785 kB     00:03
(6/12): libgomp-10.3.1-11.oe2203.x86_64.rpm                                             168 kB/s | 228 kB     00:01
(7/12): libquadmath-10.3.1-11.oe2203.x86_64.rpm                                          85 kB/s | 155 kB     00:01
(8/12): libquadmath-devel-10.3.1-11.oe2203.x86_64.rpm                                   131 kB/s |  22 kB     00:00
(9/12): libstdc++-10.3.1-11.oe2203.x86_64.rpm                                            97 kB/s | 529 kB     00:05
(10/12): gcc-gfortran-10.3.1-11.oe2203.x86_64.rpm                                       148 kB/s |  10 MB     01:09
(11/12): libstdc++-devel-10.3.1-11.oe2203.x86_64.rpm                                    124 kB/s | 2.2 MB     00:18
(12/12): gcc-10.3.1-11.oe2203.x86_64.rpm                                                167 kB/s |  29 MB     02:55
------------------------------------------------------------------------------------------------------------------------
Total                                                                                   364 kB/s |  62 MB     02:55
retrieving repo key for update unencrypted from http://repo.openeuler.org/openEuler-22.03-LTS/OS/x86_64/RPM-GPG-KEY-openEuler
update                                                                                  1.7 kB/s | 2.1 kB     00:01
Importing GPG key 0xB25E7F66:
 Userid     : "private OBS (key without passphrase) <defaultkey@localobs>"
 Fingerprint: 12EA 74AC 9DF4 8D46 C69C A0BE D557 065E B25E 7F66
 From       : http://repo.openeuler.org/openEuler-22.03-LTS/OS/x86_64/RPM-GPG-KEY-openEuler
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                1/1
  Upgrading        : libgcc-10.3.1-11.oe2203.x86_64                                                                1/24
  Running scriptlet: libgcc-10.3.1-11.oe2203.x86_64                                                                1/24
  Upgrading        : libstdc++-10.3.1-11.oe2203.x86_64                                                             2/24
  Upgrading        : libquadmath-10.3.1-11.oe2203.x86_64                                                           3/24
  Upgrading        : libgfortran-10.3.1-11.oe2203.x86_64                                                           4/24
  Upgrading        : cpp-10.3.1-11.oe2203.x86_64                                                                   5/24
  Upgrading        : libstdc++-devel-10.3.1-11.oe2203.x86_64                                                       6/24
  Upgrading        : libgomp-10.3.1-11.oe2203.x86_64                                                               7/24
  Upgrading        : gcc-10.3.1-11.oe2203.x86_64                                                                   8/24
  Upgrading        : libquadmath-devel-10.3.1-11.oe2203.x86_64                                                     9/24
  Upgrading        : gcc-gfortran-10.3.1-11.oe2203.x86_64                                                         10/24
  Upgrading        : gcc-c++-10.3.1-11.oe2203.x86_64                                                              11/24
  Upgrading        : gcc-gdb-plugin-10.3.1-11.oe2203.x86_64                                                       12/24
  Cleanup          : gcc-gfortran-10.3.1-10.oe2203.x86_64                                                         13/24
  Cleanup          : gcc-c++-10.3.1-10.oe2203.x86_64                                                              14/24
  Cleanup          : gcc-gdb-plugin-10.3.1-10.oe2203.x86_64                                                       15/24
  Cleanup          : libquadmath-devel-10.3.1-10.oe2203.x86_64                                                    16/24
  Cleanup          : gcc-10.3.1-10.oe2203.x86_64                                                                  17/24
  Cleanup          : cpp-10.3.1-10.oe2203.x86_64                                                                  18/24
  Cleanup          : libgfortran-10.3.1-10.oe2203.x86_64                                                          19/24
  Cleanup          : libstdc++-devel-10.3.1-10.oe2203.x86_64                                                      20/24
  Cleanup          : libstdc++-10.3.1-10.oe2203.x86_64                                                            21/24
  Cleanup          : libgcc-10.3.1-10.oe2203.x86_64                                                               22/24
  Running scriptlet: libgcc-10.3.1-10.oe2203.x86_64                                                               22/24
  Cleanup          : libquadmath-10.3.1-10.oe2203.x86_64                                                          23/24
  Cleanup          : libgomp-10.3.1-10.oe2203.x86_64                                                              24/24
  Running scriptlet: libgomp-10.3.1-10.oe2203.x86_64                                                              24/24
  Verifying        : cpp-10.3.1-11.oe2203.x86_64                                                                   1/24
  Verifying        : cpp-10.3.1-10.oe2203.x86_64                                                                   2/24
  Verifying        : gcc-10.3.1-11.oe2203.x86_64                                                                   3/24
  Verifying        : gcc-10.3.1-10.oe2203.x86_64                                                                   4/24
  Verifying        : gcc-c++-10.3.1-11.oe2203.x86_64                                                               5/24
  Verifying        : gcc-c++-10.3.1-10.oe2203.x86_64                                                               6/24
  Verifying        : gcc-gdb-plugin-10.3.1-11.oe2203.x86_64                                                        7/24
  Verifying        : gcc-gdb-plugin-10.3.1-10.oe2203.x86_64                                                        8/24
  Verifying        : gcc-gfortran-10.3.1-11.oe2203.x86_64                                                          9/24
  Verifying        : gcc-gfortran-10.3.1-10.oe2203.x86_64                                                         10/24
  Verifying        : libgcc-10.3.1-11.oe2203.x86_64                                                               11/24
  Verifying        : libgcc-10.3.1-10.oe2203.x86_64                                                               12/24
  Verifying        : libgfortran-10.3.1-11.oe2203.x86_64                                                          13/24
  Verifying        : libgfortran-10.3.1-10.oe2203.x86_64                                                          14/24
  Verifying        : libgomp-10.3.1-11.oe2203.x86_64                                                              15/24
  Verifying        : libgomp-10.3.1-10.oe2203.x86_64                                                              16/24
  Verifying        : libquadmath-10.3.1-11.oe2203.x86_64                                                          17/24
  Verifying        : libquadmath-10.3.1-10.oe2203.x86_64                                                          18/24
  Verifying        : libquadmath-devel-10.3.1-11.oe2203.x86_64                                                    19/24
  Verifying        : libquadmath-devel-10.3.1-10.oe2203.x86_64                                                    20/24
  Verifying        : libstdc++-10.3.1-11.oe2203.x86_64                                                            21/24
  Verifying        : libstdc++-10.3.1-10.oe2203.x86_64                                                            22/24
  Verifying        : libstdc++-devel-10.3.1-11.oe2203.x86_64                                                      23/24
  Verifying        : libstdc++-devel-10.3.1-10.oe2203.x86_64                                                      24/24

Upgraded:
  cpp-10.3.1-11.oe2203.x86_64                              gcc-10.3.1-11.oe2203.x86_64
  gcc-c++-10.3.1-11.oe2203.x86_64                          gcc-gdb-plugin-10.3.1-11.oe2203.x86_64
  gcc-gfortran-10.3.1-11.oe2203.x86_64                     libgcc-10.3.1-11.oe2203.x86_64
  libgfortran-10.3.1-11.oe2203.x86_64                      libgomp-10.3.1-11.oe2203.x86_64
  libquadmath-10.3.1-11.oe2203.x86_64                      libquadmath-devel-10.3.1-11.oe2203.x86_64
  libstdc++-10.3.1-11.oe2203.x86_64                        libstdc++-devel-10.3.1-11.oe2203.x86_64

Complete!
[root@openEuler2203 ~]#
```

​		安装g++

```bash
[root@openEuler2203 ~]# yum install -y g++
Last metadata expiration check: 0:20:11 ago on 2023年02月20日 星期一 10时50分56秒.
Package gcc-c++-10.3.1-11.oe2203.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@openEuler2203 ~]#
```



# 三、gcc的使用

## 1、基本使用格式

```bash
gcc [选项] <文件名>
```

<center>gcc常用选项</center>

| 选项    | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| -o file | 将经过gcc处理过的结果存为文件file，这个结果文件可能是预处理文件、汇编文件、目标文件或者最终的可执行文件。假设被处理的源文件为source.suffix，如果这个选项被省略了，那么生成的可执行文件默认名称为a.out；目标文件默认名为source.o；汇编文件默认名为source.s；生成的预处理文件则发送到标准输出设备。 |

<blockquote>
    我们现在看gcc的第一个语法，最简单的-o，就是输出一个文件名，然后选项对着文件名，如果不加-o的话，就是gcc直接一个文件名，它会默认输出一个文件名，我们试一下：
</blockquote>

​		先建立一个目录：

```bash
[root@openEuler2203 ~]# mkdir src
[root@openEuler2203 ~]# cd src/
[root@openEuler2203 src]#
```

​		先创建一个文件：`main.c`

```bash
[root@openEuler2203 src]# vim main.c
```

​		`main.c`：

```c
#include <stdio.h>
int main()
{
        printf("test gcc\n");
        return 0;
}
```

​		然后接续：

```bash
[root@openEuler2203 src]# ll
总用量 4.0K
-rw-r--r--. 1 root root 68  2月 20 11:20 main.c
[root@openEuler2203 src]# gcc main.c -o main
[root@openEuler2203 src]# ll
总用量 20K
-rwxr-xr-x. 1 root root 16K  2月 20 11:22 main
-rw-r--r--. 1 root root  68  2月 20 11:20 main.c
[root@openEuler2203 src]# ./main
test gcc
[root@openEuler2203 src]#
```

​		如果像直接执行的话可以把名字放到我们的环境变量当中，直接指定到`path`路径下面，如果不指定`-o`：

```bash
[root@openEuler2203 src]# gcc main.c
[root@openEuler2203 src]# ll
总用量 36K
-rwxr-xr-x. 1 root root 16K  2月 20 11:24 a.out
-rwxr-xr-x. 1 root root 16K  2月 20 11:22 main
-rw-r--r--. 1 root root  68  2月 20 11:20 main.c
[root@openEuler2203 src]# ./a.out
test gcc
[root@openEuler2203 src]#
```

+++

​		下面我们来看gcc的另一个参数`-c`，`-c`表示只编译不链接，这是我们经常会用到的一个参数，我们在编译做大的项目、包含很多文件的时候，我们不可能每次都链接的，我们在指定一个cpp编译的时候，我们一定是只让它生成。我们来看一下：

```bash
[root@openEuler2203 src]# ll
总用量 36K
-rwxr-xr-x. 1 root root 16K  2月 20 11:24 a.out
-rwxr-xr-x. 1 root root 16K  2月 20 11:32 main
-rw-r--r--. 1 root root  68  2月 20 11:20 main.c
[root@openEuler2203 src]# rm -rf main a.out
[root@openEuler2203 src]# ll
总用量 4.0K
-rw-r--r--. 1 root root 68  2月 20 11:20 main.c
[root@openEuler2203 src]# gcc -c main.c
[root@openEuler2203 src]# ll
总用量 8.0K
-rw-r--r--. 1 root root   68  2月 20 11:20 main.c
-rw-r--r--. 1 root root 1.5K  2月 20 11:33 main.o
[root@openEuler2203 src]#
```

​		这样的话它就生成了一个`.o`文件，这个时候我们可以分开来再做一件事：

```bash
[root@openEuler2203 src]# gcc main.o -o main
[root@openEuler2203 src]# ll
总用量 24K
-rwxr-xr-x. 1 root root  16K  2月 20 11:34 main
-rw-r--r--. 1 root root   68  2月 20 11:20 main.c
-rw-r--r--. 1 root root 1.5K  2月 20 11:33 main.o
[root@openEuler2203 src]# ./main
test gcc
[root@openEuler2203 src]#
```

​		这是分成了两部做的，最终生成了这个`main`。我们分了两个步骤，第一步是先编译不链接，生成了`.o`文件，然后第二步我们再编译再进行链接，生成了执行文件，其实为什么要这样做呢？你在编`.c`文件的时候，每一个`.c`文件对应的头文件都是单独编译的，就算你写了多个文件之后，它也会把它切割进行单独编译，单独编译的每一个`.c`文件可能会有不同的头文件参数、会有很多设置，比如说每个`.c`一般都是要`-c`的，最终在做链接，因为你在做编译的时候，你有些参数是不需要加的，比方说，库文件的路径，你用的库，你在编译的时候是不用考虑的，而只有在链接的时候才要加，这样我们把一些参数配置分在各个地方进行处理。

​		我们继续在说几个参数：`gcc -E`，就是预编译：

```bash
[root@openEuler2203 src]# gcc -E main.c
# 1 "main.c"
# 1 "<built-in>"
# 1 "<命令行>"
# 31 "<命令行>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 32 "<命令行>" 2
# 1 "main.c"
# 1 "/usr/include/stdio.h" 1 3 4
# 27 "/usr/include/stdio.h" 3 4
# 1 "/usr/include/bits/libc-header-start.h" 1 3 4
# 33 "/usr/include/bits/libc-header-start.h" 3 4
# 1 "/usr/include/features.h" 1 3 4
# 392 "/usr/include/features.h" 3 4
# 1 "/usr/include/features-time64.h" 1 3 4
# 20 "/usr/include/features-time64.h" 3 4
# 1 "/usr/include/bits/wordsize.h" 1 3 4
# 21 "/usr/include/features-time64.h" 2 3 4
# 1 "/usr/include/bits/timesize.h" 1 3 4
# 22 "/usr/include/features-time64.h" 2 3 4
# 393 "/usr/include/features.h" 2 3 4
# 488 "/usr/include/features.h" 3 4
# 1 "/usr/include/sys/cdefs.h" 1 3 4
# 499 "/usr/include/sys/cdefs.h" 3 4
# 1 "/usr/include/bits/wordsize.h" 1 3 4
# 500 "/usr/include/sys/cdefs.h" 2 3 4
# 1 "/usr/include/bits/long-double.h" 1 3 4
# 501 "/usr/include/sys/cdefs.h" 2 3 4
# 489 "/usr/include/features.h" 2 3 4
# 512 "/usr/include/features.h" 3 4
# 1 "/usr/include/gnu/stubs.h" 1 3 4
# 10 "/usr/include/gnu/stubs.h" 3 4
# 1 "/usr/include/gnu/stubs-64.h" 1 3 4
# 11 "/usr/include/gnu/stubs.h" 2 3 4
# 513 "/usr/include/features.h" 2 3 4
# 34 "/usr/include/bits/libc-header-start.h" 2 3 4
# 28 "/usr/include/stdio.h" 2 3 4





# 1 "/usr/lib/gcc/x86_64-linux-gnu/10.3.1/include/stddef.h" 1 3 4
# 209 "/usr/lib/gcc/x86_64-linux-gnu/10.3.1/include/stddef.h" 3 4

# 209 "/usr/lib/gcc/x86_64-linux-gnu/10.3.1/include/stddef.h" 3 4
typedef long unsigned int size_t;
# 34 "/usr/include/stdio.h" 2 3 4


# 1 "/usr/lib/gcc/x86_64-linux-gnu/10.3.1/include/stdarg.h" 1 3 4
# 40 "/usr/lib/gcc/x86_64-linux-gnu/10.3.1/include/stdarg.h" 3 4
typedef __builtin_va_list __gnuc_va_list;
# 37 "/usr/include/stdio.h" 2 3 4

# 1 "/usr/include/bits/types.h" 1 3 4
# 27 "/usr/include/bits/types.h" 3 4
# 1 "/usr/include/bits/wordsize.h" 1 3 4
# 28 "/usr/include/bits/types.h" 2 3 4
# 1 "/usr/include/bits/timesize.h" 1 3 4
# 29 "/usr/include/bits/types.h" 2 3 4


typedef unsigned char __u_char;
typedef unsigned short int __u_short;
typedef unsigned int __u_int;
typedef unsigned long int __u_long;


typedef signed char __int8_t;
typedef unsigned char __uint8_t;
typedef signed short int __int16_t;
typedef unsigned short int __uint16_t;
typedef signed int __int32_t;
typedef unsigned int __uint32_t;

typedef signed long int __int64_t;
typedef unsigned long int __uint64_t;






typedef __int8_t __int_least8_t;
typedef __uint8_t __uint_least8_t;
typedef __int16_t __int_least16_t;
typedef __uint16_t __uint_least16_t;
typedef __int32_t __int_least32_t;
typedef __uint32_t __uint_least32_t;
typedef __int64_t __int_least64_t;
typedef __uint64_t __uint_least64_t;



typedef long int __quad_t;
typedef unsigned long int __u_quad_t;







typedef long int __intmax_t;
typedef unsigned long int __uintmax_t;
# 141 "/usr/include/bits/types.h" 3 4
# 1 "/usr/include/bits/typesizes.h" 1 3 4
# 142 "/usr/include/bits/types.h" 2 3 4
# 1 "/usr/include/bits/time64.h" 1 3 4
# 143 "/usr/include/bits/types.h" 2 3 4


typedef unsigned long int __dev_t;
typedef unsigned int __uid_t;
typedef unsigned int __gid_t;
typedef unsigned long int __ino_t;
typedef unsigned long int __ino64_t;
typedef unsigned int __mode_t;
typedef unsigned long int __nlink_t;
typedef long int __off_t;
typedef long int __off64_t;
typedef int __pid_t;
typedef struct { int __val[2]; } __fsid_t;
typedef long int __clock_t;
typedef unsigned long int __rlim_t;
typedef unsigned long int __rlim64_t;
typedef unsigned int __id_t;
typedef long int __time_t;
typedef unsigned int __useconds_t;
typedef long int __suseconds_t;
typedef long int __suseconds64_t;

typedef int __daddr_t;
typedef int __key_t;


typedef int __clockid_t;


typedef void * __timer_t;


typedef long int __blksize_t;




typedef long int __blkcnt_t;
typedef long int __blkcnt64_t;


typedef unsigned long int __fsblkcnt_t;
typedef unsigned long int __fsblkcnt64_t;


typedef unsigned long int __fsfilcnt_t;
typedef unsigned long int __fsfilcnt64_t;


typedef long int __fsword_t;

typedef long int __ssize_t;


typedef long int __syscall_slong_t;

typedef unsigned long int __syscall_ulong_t;



typedef __off64_t __loff_t;
typedef char *__caddr_t;


typedef long int __intptr_t;


typedef unsigned int __socklen_t;




typedef int __sig_atomic_t;
# 39 "/usr/include/stdio.h" 2 3 4
# 1 "/usr/include/bits/types/__fpos_t.h" 1 3 4




# 1 "/usr/include/bits/types/__mbstate_t.h" 1 3 4
# 13 "/usr/include/bits/types/__mbstate_t.h" 3 4
typedef struct
{
  int __count;
  union
  {
    unsigned int __wch;
    char __wchb[4];
  } __value;
} __mbstate_t;
# 6 "/usr/include/bits/types/__fpos_t.h" 2 3 4




typedef struct _G_fpos_t
{
  __off_t __pos;
  __mbstate_t __state;
} __fpos_t;
# 40 "/usr/include/stdio.h" 2 3 4
# 1 "/usr/include/bits/types/__fpos64_t.h" 1 3 4
# 10 "/usr/include/bits/types/__fpos64_t.h" 3 4
typedef struct _G_fpos64_t
{
  __off64_t __pos;
  __mbstate_t __state;
} __fpos64_t;
# 41 "/usr/include/stdio.h" 2 3 4
# 1 "/usr/include/bits/types/__FILE.h" 1 3 4



struct _IO_FILE;
typedef struct _IO_FILE __FILE;
# 42 "/usr/include/stdio.h" 2 3 4
# 1 "/usr/include/bits/types/FILE.h" 1 3 4



struct _IO_FILE;


typedef struct _IO_FILE FILE;
# 43 "/usr/include/stdio.h" 2 3 4
# 1 "/usr/include/bits/types/struct_FILE.h" 1 3 4
# 35 "/usr/include/bits/types/struct_FILE.h" 3 4
struct _IO_FILE;
struct _IO_marker;
struct _IO_codecvt;
struct _IO_wide_data;




typedef void _IO_lock_t;





struct _IO_FILE
{
  int _flags;


  char *_IO_read_ptr;
  char *_IO_read_end;
  char *_IO_read_base;
  char *_IO_write_base;
  char *_IO_write_ptr;
  char *_IO_write_end;
  char *_IO_buf_base;
  char *_IO_buf_end;


  char *_IO_save_base;
  char *_IO_backup_base;
  char *_IO_save_end;

  struct _IO_marker *_markers;

  struct _IO_FILE *_chain;

  int _fileno;
  int _flags2;
  __off_t _old_offset;


  unsigned short _cur_column;
  signed char _vtable_offset;
  char _shortbuf[1];

  _IO_lock_t *_lock;







  __off64_t _offset;

  struct _IO_codecvt *_codecvt;
  struct _IO_wide_data *_wide_data;
  struct _IO_FILE *_freeres_list;
  void *_freeres_buf;
  size_t __pad5;
  int _mode;

  char _unused2[15 * sizeof (int) - 4 * sizeof (void *) - sizeof (size_t)];
};
# 44 "/usr/include/stdio.h" 2 3 4
# 52 "/usr/include/stdio.h" 3 4
typedef __gnuc_va_list va_list;
# 63 "/usr/include/stdio.h" 3 4
typedef __off_t off_t;
# 77 "/usr/include/stdio.h" 3 4
typedef __ssize_t ssize_t;






typedef __fpos_t fpos_t;
# 133 "/usr/include/stdio.h" 3 4
# 1 "/usr/include/bits/stdio_lim.h" 1 3 4
# 134 "/usr/include/stdio.h" 2 3 4



extern FILE *stdin;
extern FILE *stdout;
extern FILE *stderr;






extern int remove (const char *__filename) __attribute__ ((__nothrow__ , __leaf__));

extern int rename (const char *__old, const char *__new) __attribute__ ((__nothrow__ , __leaf__));



extern int renameat (int __oldfd, const char *__old, int __newfd,
       const char *__new) __attribute__ ((__nothrow__ , __leaf__));
# 172 "/usr/include/stdio.h" 3 4
extern int fclose (FILE *__stream);
# 182 "/usr/include/stdio.h" 3 4
extern FILE *tmpfile (void)
  __attribute__ ((__malloc__)) ;
# 199 "/usr/include/stdio.h" 3 4
extern char *tmpnam (char[20]) __attribute__ ((__nothrow__ , __leaf__)) ;




extern char *tmpnam_r (char __s[20]) __attribute__ ((__nothrow__ , __leaf__)) ;
# 216 "/usr/include/stdio.h" 3 4
extern char *tempnam (const char *__dir, const char *__pfx)
   __attribute__ ((__nothrow__ , __leaf__)) __attribute__ ((__malloc__)) ;






extern int fflush (FILE *__stream);
# 233 "/usr/include/stdio.h" 3 4
extern int fflush_unlocked (FILE *__stream);
# 252 "/usr/include/stdio.h" 3 4
extern FILE *fopen (const char *__restrict __filename,
      const char *__restrict __modes)
  __attribute__ ((__malloc__)) ;




extern FILE *freopen (const char *__restrict __filename,
        const char *__restrict __modes,
        FILE *__restrict __stream) ;
# 287 "/usr/include/stdio.h" 3 4
extern FILE *fdopen (int __fd, const char *__modes) __attribute__ ((__nothrow__ , __leaf__))
  __attribute__ ((__malloc__)) ;
# 302 "/usr/include/stdio.h" 3 4
extern FILE *fmemopen (void *__s, size_t __len, const char *__modes)
  __attribute__ ((__nothrow__ , __leaf__)) __attribute__ ((__malloc__)) ;




extern FILE *open_memstream (char **__bufloc, size_t *__sizeloc) __attribute__ ((__nothrow__ , __leaf__))
  __attribute__ ((__malloc__)) ;
# 322 "/usr/include/stdio.h" 3 4
extern void setbuf (FILE *__restrict __stream, char *__restrict __buf) __attribute__ ((__nothrow__ , __leaf__));



extern int setvbuf (FILE *__restrict __stream, char *__restrict __buf,
      int __modes, size_t __n) __attribute__ ((__nothrow__ , __leaf__));




extern void setbuffer (FILE *__restrict __stream, char *__restrict __buf,
         size_t __size) __attribute__ ((__nothrow__ , __leaf__));


extern void setlinebuf (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));







extern int fprintf (FILE *__restrict __stream,
      const char *__restrict __format, ...);




extern int printf (const char *__restrict __format, ...);

extern int sprintf (char *__restrict __s,
      const char *__restrict __format, ...) __attribute__ ((__nothrow__));





extern int vfprintf (FILE *__restrict __s, const char *__restrict __format,
       __gnuc_va_list __arg);




extern int vprintf (const char *__restrict __format, __gnuc_va_list __arg);

extern int vsprintf (char *__restrict __s, const char *__restrict __format,
       __gnuc_va_list __arg) __attribute__ ((__nothrow__));



extern int snprintf (char *__restrict __s, size_t __maxlen,
       const char *__restrict __format, ...)
     __attribute__ ((__nothrow__)) __attribute__ ((__format__ (__printf__, 3, 4)));

extern int vsnprintf (char *__restrict __s, size_t __maxlen,
        const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__nothrow__)) __attribute__ ((__format__ (__printf__, 3, 0)));
# 397 "/usr/include/stdio.h" 3 4
extern int vdprintf (int __fd, const char *__restrict __fmt,
       __gnuc_va_list __arg)
     __attribute__ ((__format__ (__printf__, 2, 0)));
extern int dprintf (int __fd, const char *__restrict __fmt, ...)
     __attribute__ ((__format__ (__printf__, 2, 3)));







extern int fscanf (FILE *__restrict __stream,
     const char *__restrict __format, ...) ;




extern int scanf (const char *__restrict __format, ...) ;

extern int sscanf (const char *__restrict __s,
     const char *__restrict __format, ...) __attribute__ ((__nothrow__ , __leaf__));





# 1 "/usr/include/bits/floatn.h" 1 3 4
# 119 "/usr/include/bits/floatn.h" 3 4
# 1 "/usr/include/bits/floatn-common.h" 1 3 4
# 24 "/usr/include/bits/floatn-common.h" 3 4
# 1 "/usr/include/bits/long-double.h" 1 3 4
# 25 "/usr/include/bits/floatn-common.h" 2 3 4
# 120 "/usr/include/bits/floatn.h" 2 3 4
# 425 "/usr/include/stdio.h" 2 3 4



extern int fscanf (FILE *__restrict __stream, const char *__restrict __format, ...) __asm__ ("" "__isoc99_fscanf")

                               ;
extern int scanf (const char *__restrict __format, ...) __asm__ ("" "__isoc99_scanf")
                              ;
extern int sscanf (const char *__restrict __s, const char *__restrict __format, ...) __asm__ ("" "__isoc99_sscanf") __attribute__ ((__nothrow__ , __leaf__))

                      ;
# 453 "/usr/include/stdio.h" 3 4
extern int vfscanf (FILE *__restrict __s, const char *__restrict __format,
      __gnuc_va_list __arg)
     __attribute__ ((__format__ (__scanf__, 2, 0))) ;





extern int vscanf (const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__format__ (__scanf__, 1, 0))) ;


extern int vsscanf (const char *__restrict __s,
      const char *__restrict __format, __gnuc_va_list __arg)
     __attribute__ ((__nothrow__ , __leaf__)) __attribute__ ((__format__ (__scanf__, 2, 0)));





extern int vfscanf (FILE *__restrict __s, const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vfscanf")



     __attribute__ ((__format__ (__scanf__, 2, 0))) ;
extern int vscanf (const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vscanf")

     __attribute__ ((__format__ (__scanf__, 1, 0))) ;
extern int vsscanf (const char *__restrict __s, const char *__restrict __format, __gnuc_va_list __arg) __asm__ ("" "__isoc99_vsscanf") __attribute__ ((__nothrow__ , __leaf__))



     __attribute__ ((__format__ (__scanf__, 2, 0)));
# 507 "/usr/include/stdio.h" 3 4
extern int fgetc (FILE *__stream);
extern int getc (FILE *__stream);





extern int getchar (void);






extern int getc_unlocked (FILE *__stream);
extern int getchar_unlocked (void);
# 532 "/usr/include/stdio.h" 3 4
extern int fgetc_unlocked (FILE *__stream);
# 543 "/usr/include/stdio.h" 3 4
extern int fputc (int __c, FILE *__stream);
extern int putc (int __c, FILE *__stream);





extern int putchar (int __c);
# 559 "/usr/include/stdio.h" 3 4
extern int fputc_unlocked (int __c, FILE *__stream);







extern int putc_unlocked (int __c, FILE *__stream);
extern int putchar_unlocked (int __c);






extern int getw (FILE *__stream);


extern int putw (int __w, FILE *__stream);







extern char *fgets (char *__restrict __s, int __n, FILE *__restrict __stream)
     __attribute__ ((__access__ (__write_only__, 1, 2)));
# 626 "/usr/include/stdio.h" 3 4
extern __ssize_t __getdelim (char **__restrict __lineptr,
                             size_t *__restrict __n, int __delimiter,
                             FILE *__restrict __stream) ;
extern __ssize_t getdelim (char **__restrict __lineptr,
                           size_t *__restrict __n, int __delimiter,
                           FILE *__restrict __stream) ;







extern __ssize_t getline (char **__restrict __lineptr,
                          size_t *__restrict __n,
                          FILE *__restrict __stream) ;







extern int fputs (const char *__restrict __s, FILE *__restrict __stream);





extern int puts (const char *__s);






extern int ungetc (int __c, FILE *__stream);






extern size_t fread (void *__restrict __ptr, size_t __size,
       size_t __n, FILE *__restrict __stream) ;




extern size_t fwrite (const void *__restrict __ptr, size_t __size,
        size_t __n, FILE *__restrict __s);
# 696 "/usr/include/stdio.h" 3 4
extern size_t fread_unlocked (void *__restrict __ptr, size_t __size,
         size_t __n, FILE *__restrict __stream) ;
extern size_t fwrite_unlocked (const void *__restrict __ptr, size_t __size,
          size_t __n, FILE *__restrict __stream);







extern int fseek (FILE *__stream, long int __off, int __whence);




extern long int ftell (FILE *__stream) ;




extern void rewind (FILE *__stream);
# 730 "/usr/include/stdio.h" 3 4
extern int fseeko (FILE *__stream, __off_t __off, int __whence);




extern __off_t ftello (FILE *__stream) ;
# 754 "/usr/include/stdio.h" 3 4
extern int fgetpos (FILE *__restrict __stream, fpos_t *__restrict __pos);




extern int fsetpos (FILE *__stream, const fpos_t *__pos);
# 780 "/usr/include/stdio.h" 3 4
extern void clearerr (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));

extern int feof (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;

extern int ferror (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;



extern void clearerr_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
extern int feof_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
extern int ferror_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;







extern void perror (const char *__s);




extern int fileno (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;




extern int fileno_unlocked (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;
# 817 "/usr/include/stdio.h" 3 4
extern int pclose (FILE *__stream);





extern FILE *popen (const char *__command, const char *__modes)
  __attribute__ ((__malloc__)) ;






extern char *ctermid (char *__s) __attribute__ ((__nothrow__ , __leaf__))
  __attribute__ ((__access__ (__write_only__, 1)));
# 861 "/usr/include/stdio.h" 3 4
extern void flockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));



extern int ftrylockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__)) ;


extern void funlockfile (FILE *__stream) __attribute__ ((__nothrow__ , __leaf__));
# 879 "/usr/include/stdio.h" 3 4
extern int __uflow (FILE *);
extern int __overflow (FILE *, int);
# 896 "/usr/include/stdio.h" 3 4

# 2 "main.c" 2

# 2 "main.c"
int main()
{
 printf("test gcc\n");
 return 0;
}
[root@openEuler2203 src]#
```

​		预编译它会把所有的头文件和宏拼到一个`.c`文件当中，也就是其实对于gcc来说，你一个`cpp`编译，它最终会把你所有的文件拼到一个文件里，因为它最终编译只编到这一个文件，它有这样一个预编译的过程，会把你所有的宏进行替换，把头文件靠过来，用宏进行替换，最终生成了一个完整的代码，包含所有头文件。

​		看到最后还是文件的结尾部分，把所有的引用头都加进来，这就是`-E`。

​		我们再看一个参数：`gcc -S`：只编译不汇编

```bash
[root@openEuler2203 src]# ll
总用量 24K
-rwxr-xr-x. 1 root root  16K  2月 20 11:34 main
-rw-r--r--. 1 root root   68  2月 20 11:20 main.c
-rw-r--r--. 1 root root 1.5K  2月 20 11:33 main.o
[root@openEuler2203 src]# gcc -S main.c
[root@openEuler2203 src]# ll
总用量 28K
-rwxr-xr-x. 1 root root  16K  2月 20 11:34 main
-rw-r--r--. 1 root root   68  2月 20 11:20 main.c
-rw-r--r--. 1 root root 1.5K  2月 20 11:33 main.o
-rw-r--r--. 1 root root  424  2月 20 11:49 main.s
[root@openEuler2203 src]#
```

​		你看到它会生成一个`main.s`的文件，我们打开看一下：

```bash
[root@openEuler2203 src]# cat main.s
        .file   "main.c"
        .text
        .section        .rodata
.LC0:
        .string "test gcc"
        .text
        .globl  main
        .type   main, @function
main:
.LFB0:
        .cfi_startproc
        pushq   %rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        movq    %rsp, %rbp
        .cfi_def_cfa_register 6
        movl    $.LC0, %edi
        call    puts
        movl    $0, %eax
        popq    %rbp
        .cfi_def_cfa 7, 8
        ret
        .cfi_endproc
.LFE0:
        .size   main, .-main
        .ident  "GCC: (GNU) 10.3.1"
        .section        .note.GNU-stack,"",@progbits
[root@openEuler2203 src]#
```

​		这里面对应的就是汇编代码，这就是有利于我们在做检查和学习你的代码是怎么生成相应的汇编，因为各个机器上面它是不一样的，比方说我们这个是64位的，跟x86的，还有就是我们用的很多的都是arm机器上的，它生成的汇编码是不一致的，所以我们可以通过这个来学习包括你怎么去提高效率。

​		我们再看一下如何生成调试信息出来：

```bash
[root@openEuler2203 src]# gcc -g main.c -o main_d
[root@openEuler2203 src]# ll
总用量 48K
-rwxr-xr-x. 1 root root  16K  2月 20 11:34 main
-rw-r--r--. 1 root root   68  2月 20 11:20 main.c
-rwxr-xr-x. 1 root root  17K  2月 20 11:55 main_d
-rw-r--r--. 1 root root 1.5K  2月 20 11:33 main.o
-rw-r--r--. 1 root root  424  2月 20 11:49 main.s
[root@openEuler2203 src]#
```

​		可以看一下，我们生成的这个文件，`main_d`加了调试信息会比普通的文件会大一点，其实也就对应像是在Windows上编程的话，就会有一个release版本，和debug版本，相当于这种默认的就是release版本，像`main_d`的就是debug版本。debug版本里面会加入你的调试信息。
