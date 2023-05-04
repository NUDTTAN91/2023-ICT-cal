<center><font size=7><b>openEuler防火墙和安全加固</b></font></center>

+++

[TOC]

+++

# 一、传统RWX安全机制

## 1、系统权限

### （1）安装系统

- 选择稳定版操作系统
- 最小化安装
- 不要安装gcc、make
- 安装完系统后更新系统

```bash
[root@openEuler ~]# yum -y update
```

### （2）基本权限`R` `W` `X`

- 对于目录，默认权限=777-umask
- 对于文件，默认权限=666-umask（文件默认无执行权限）
- 修改umask

### （3）特殊权限`suid` `sgid` `sticky`

- `suid`冒险位，执行二进制文件与文件所有人有关，与谁来执行无关

```bash
[root@openEuler ~]# chmod 4777 filename
```

- `sgid`强制位，对于目录生效，在此目录种创建文件自动归入目录所在组

```bash
[root@openEuler ~]# chmod 2777 dirname
```

- `stick`粘制位，目录种的文件只能被文件拥有者删除

```bash
[root@openEuler ~]# chmod 1777 dirname
```



# 二、防火墙

## 1、防火墙的作用

![image-20230118104210068](.\image\image-20230118104210068.png)

​		在计算机领域，防火墙是用于保护信息安全的设备，其会依照用户定义的规则，允许或限制数据的传输。

![image-20230118104341330](.\image\image-20230118104341330.png)

总结：

- 用于保护内网安全的一种设备
- 依据规则进行防护
- 用户定义规则
- 允许或拒绝外部用户访问

## 2、防火墙的分类

- 逻辑上划分，防火墙可以大体分为主机防火墙和网络防火墙\ 主机防火墙：针对于单个主机进行防护\ 网络防火墙：针对网络进行防护，处于网络边缘，防火墙背后是本地局域网\ 网络防火墙主外(服务集体)，主机防火墙主内(服务个人)
- 物理上划分，防火墙可分为硬件防火墙和软件防火墙 硬件防火墙：在硬件级别实现防火墙功能，另一部分基于软件实现，其性能高，硬件成本高
  软件防火墙：应用软件处理逻辑运行于通用硬件平台之上的防火墙，其性能相较于硬件防火墙低，成本较低，对于 Linux系统已自带，直接使用即可

## 3、防火墙性能

- 吞吐量
- 并发连接
- 新建连接
- 时延
- 抖动

## 4、硬件防火墙

### （1）硬件防火墙的定义

​		硬件防火墙是指把具备配置数据包通过规则的软件嵌入硬件设备中，为网络提供安全防护的硬件设备。多见于网络边缘。

![image-20230118105558373](.\image\image-20230118105558373.png)

### （2）硬件防火墙品牌

- Juniper

![image-20230118105732770](.\image\image-20230118105732770.png)

- cisco 思科ASA

![image-20230118105802442](.\image\image-20230118105802442.png)

- 华为

![image-20230118105824570](.\image\image-20230118105824570.png)

总结：

- 什么是硬件防火墙？
  - 软件嵌入到硬件设备种
- 硬件防火墙处于网络边缘
- 硬件防火墙品牌：juniper、cisco、华为

## 5、软件防火墙

​		软件防火墙是单独使用具备配置数据包通过规则的软件来实现数据包过滤。多见于单主机系统或个人计算机。

![image-20230118110800602](.\image\image-20230118110800602.png)

总结：

- 通过软件配置规则，允许或拒绝数据包通过
- 个人主机，托管于机房的服务器，云平台开启安全组

## 6、扩展：Web应用防火墙(WAF)

​		Web应用防火墙是对web防护(网页保护)的安全防护设备(软件)，主要用于截获所有HTTP数据或仅仅满足某些规则的 会话。多见于云平台中。

![image-20230118111121947](.\image\image-20230118111121947.png)

## 7、硬件防火墙与软件防火墙比较

​		硬件防火墙有独立的硬件设备，运算效率较高，价格略高，可为计算机网络提供安全防护。

​		软件防火墙必须部署在主机系统之上，相较于硬件防火墙运算效率低，在一定程度上会影响到主机系统性能，一般用 于单机系统或个人计算机中，不直接用于计算机网络中。



# 三、iptables

## 1、iptables是什么？

- iptables不是防火墙，是防火墙用户代理
- 用于把用户的安全设置添加到"安全框架"中
- "安全框架"是防火墙
- "安全框架"的名称为netﬁlter netﬁlter位于内核空间中，是Linux操作系统核心层内部的一个数据包处理模块
- iptables是用于在用户空间对内核空间的netﬁlter进行操作的命令行工具

## 2、netfilter/iptables功能

​		netﬁlter/iptables可简称为iptables，为Linux平台下的包过滤防火墙，是开源的，内核自带的，可以代替成本较高的 企业级硬件防火墙，能够实现如下功能：

- 数据包过滤，即防火墙
- 数据包重定向，即转发
- 网络地址转换，即可NAT

注：
		**平常我们使用iptables并不是防火墙的"服务"，而服务是由内核提供的。**

总结：

- 数据包过滤，是防火墙
- 数据包重定向，是转发
- 网络地址转换，是NAT

## 3、iptable的概念

### （1）iptable工作依据——规则（rules）

​		ptables工作依据------规则(rules) iptables是按照规则(rules)来办事的，而规则就是运维人员所定义的条件；规则一般定义为"如果数据包头符合这样的 条件，就这样处理这个数据包"。

​		规则存储在内核空间的数据包过滤表中，这些规则分别指定了源地址、目的地址，传输协议(TCP、UDP、ICMP)和服 务类型(HTTP、FTP)等。

​		当数据包与规则匹配时，iptables就根据规则所定义的方法来处理这些数据包，比如放行(ACCEPT)、拒绝(REJECT)、 丢弃(DROP)等
<blockquote>
    配置防火墙主要工作就是对iptables规则进行添加、修改、删除等
</blockquote>

总结：

- 工作依据是规则
- 规则存在表中
- 规则指定了什么？源地址或目的地址或传输协议或服务类型
- 如果数据包被匹配到了，则按规则指定的动作去执行

## 4、iptables中链的概念

​		举例说明：
​		当客户端访问服务器端的web服务时，客户端发送访问请求报文至网卡，而tcp/ip协议栈是属于内核的一部分，所 以，客户端的请求报文会通过内核的TCP协议传输到用户空间的web服务，而客户端报文的目标地址为web服务器所 监听的套接字(ip:port)上，当web服务器响应客户端请求时，web服务所回应的响应报文的目标地址为客户端地址， 我们说过，netﬁlter才是真正的防火墙，属于内核的一部分，所以，我们要想让netﬁlter起到作用，我们就需要在内 核中设置"关口"，所以进出的数据报文都要通过这些关口，经检查，符合放行条件的准允放行，符合阻拦条件的则被 阻止，于是就出现了input和output关口，然而在iptables中我们把关口叫做"链"。

![image-20230118112239447](.\image\image-20230118112239447.png)

​		上面的举例中，如果客户端发到本机的报文中包含的服务器地址并不是本机，而是其他服务器，此时本机就应该能够 进行转发，那么这个转发就是本机内核所支持的IP_FORWARD功能，此时我们的主机类似于路由器功能，所以我们会 看到在iptables中，所谓的关口并只有上面所提到的`input`及`output`这两个，应该还有`路由前`，`转发`，`路由后`。

​		它们所对应的英文名称分别为`PREROUTING`，`FORWARD`，`POSTROUTING`，这就是我们说到的5链。
![image-20230118112418676](.\image\image-20230118112418676.png)

​		通过上图可以看出，当我们在本地启动了防火墙功能时，数据报文需要经过以上关口，根据各报文情况，各报名经常 的"链"可能不同，如果报文目标地址是本机，则会经常input链发往本机用户空间，如果报文目标不是本机，则会直接 在内核空间中经常`forward`链和`postrouting`链转发出去。

​		有的时候我们也经常听到人们在称呼`input`为`规则链`，这又是怎么回事呢？我们知道，防火墙的作用在于对经过的数 据报文进行规则匹配，然后执行对应的"动作"，所以数据包经过这些关口时，必须匹配这个关口规则，但是关口规则 可能不止一条，可能会有很多，当我们把众多规则放在一个关口上时，所有的数据包经常都要进行匹配，那么就形成 了一个要匹配的规则链条，因此我们也把"链"称作"规则链"。
![image-20230118112507372](.\image\image-20230118112507372.png)

- `INPUT`:处理入站数据包
- `OUTPUT`:处理出站数据包
- `FORWARD`:处理转发数据包(主要是将数据包转发至本机其它网卡) 当数据报文经过本机时，网卡接收数据报文至缓冲区，内核读取报文ip首部，发现报文不是送到本机时（目的ip 不是本机），由内核直接送到forward链做匹配，匹配之后若符合forward的规则，再经由postrouting送往下一 跳或目的主机。
- `PREROUTING`:在进行路由选择前处理数据包，修改到达防火墙数据包的目的IP地址，用于判断目标主机.
- `POSTROUTING`:在进行路由选择后处理数据包，修改要离开防火墙数据包的源IP地址，判断经由哪一接口送往 下一跳.

## 5、iptables中表的概念

​		每个"规则链"上都设置了一串规则，这样的话，我们就可以把不同的"规则链"组合成能够完成某一特定功能集合分 类，而这个集合分类我们就称为表，iptables中共有5张表，学习iptables需要搞明白每种表的作用。

- ﬁlter: 过滤功能，确定是否放行该数据包，属于真正防火墙，内核模块：iptables_ﬁlter
- nat: 网络地址转换功能，修改数据包中的源、目标IP地址或端口；内核模块：iptable_nat
- mangle: 对数据包进行重新封装功能，为数据包设置标记；内核模块：iptable_mangle
- raw: 确定是否对数据包进行跟踪；内核模块：iptables_raw
- security:是否定义强制访问控制规则；后加上的

## 6、iptables中表链之间的关系

​		iptables中表链之间的关系 我们在应用防火墙时是以表为操作入口的，只要在相应的表中的规则链上添加规则即可实现某一功能。那么我们就应 该知道哪张表包括哪些规则链，然后在规则链上操作即可。

- ﬁlter表可以使用哪些链定义规则：INPUT,FORWARD,OUTPUT

- nat表中可以使用哪些链定义规则：PREROUTING,OUTPUT ,POSTROUTING,INPUT

- mangle 表中可以使用哪些链定义规则：

- PREROUTING,INPUT,FORWARD,OUTPUT,POSTROUTING

- raw表中可以使用哪些链定义规则：PREROUTING,OUTPUT

![image-20230118112945251](.\image\image-20230118112945251.png)

总结：

- 什么是表？能够完成某特定功能的链的集合

- 有几张？filter nat mangle raw security

- 表是操作防火墙的入口，如果定义规则则选择表及选择链

## 7、iptables中表的优先级

​		raw-mangle-nat-ﬁlter(由高至低)

## 8、数据包流经iptables流程

![image-20230118113159311](.\image\image-20230118113159311.png)

## 9、iptables规则匹配及动作

​		规则：根据指定的匹配条件来尝试匹配每个流经此处的数据包，匹配成功，则由规则指定的处理动作进行处理。 规则是由匹配条件和动作组成的，那么规则是什么呢？

​		举例说明： 两个同学，一个白头发，一个黑头发，同时进教室，而进教室的条件是只有黑头发可以进入，白头发拒绝进入，黑头 发和白头发就是匹配条件，而可以进入和拒绝进入就是动作。
## 10、iptables规则匹配条件分类

基本匹配条件：

​		源地址，目标地址，源端口，目标端口等

基本匹配使用选项及功能

```bash
-p 指定规则协议，tcp udp icmp all
-s 指定数据包的源地址，ip hostname
-d 指定目的地址
-i 输入接口
-o 输出接口                                              
 ! 取反
```

​		基本匹配的特点是：无需加载扩展模块，匹配规则生效

​		扩展匹配条件：

​		扩展匹配又分为显示匹配和隐式匹配。

​		扩展匹配的特点是：需要加载扩展模块，匹配规则方可生效。
​		隐式匹配的特点：使用-p选项指明协议时，无需再同时使用-m选项指明扩展模块以及不需要手动加载扩展模块； 显示匹配的特点：必须使用-m选项指明要调用的扩展模块的扩展机制以及需要手动加载扩展模块。

​		隐式匹配选项及功能

```bash
-p tcp
	--sport 匹配报文源端口；可以给出多个端口，但只能是连续的端口范围
	--dport 匹配报文目标端口；可以给出多个端口，但只能是连续的端口范围
	--tcp-flags mask comp 匹配报文中的tcp协议的标志位

-p udp
	--sport 匹配报文源端口；可以给出多个端口，但只能是连续的端口范围
	--dport 匹配报文目标端口；可以给出多个端口，但只能是连续的端口范围

--icmp-type
 0/0： echo reply 允许其他主机ping\
 8/0：echo request 允许ping其他主机
```

​		显式匹配使用选项及功能

- multiport多端口

```bash
iptables -I INPUT -d [192.168.2.10](http://192.168.2.10) -p tcp -m multiport --dports 22,80 -j ACCEPT #在INPUT链中开放本机tcp 22，tcp80端口

iptables -I OUTPUT -s [192.168.2.10](http://192.168.2.10) -p tcp -m multiport --sports 22,80 -j ACCEPT #在OUTPUT链中开发源端口tcp 22，tcp80
```

- iprange 多ip地址

```bash
iptables -A INPUT -d [192.168.2.10](http://192.168.2.10) -p tcp --dport 23 -m iprange --src-range 192.168.2.11-192.168.2.21 -j ACCEPT

iptables -A OUTPUT -s [192.168.2.10](http://192.168.2.10) -p tcp --sport 23 -m iprange --dst-range 192.168.2.11-192.168.2.21 -j ACCEPT
```

- time 指定访问时间范围

```bash
iptables -A INPUT -d [192.168.2.10](http://192.168.2.10) -p tcp \--dport 901 -m time \--weekdays Mon,Tus,Wed,Thu,Fri \--timestart 08:00:00 \--time-stop 18:00:00 -j ACCEPT

iptables -A OUTPUT -s [192.168.2.10](http://192.168.2.10) -p tcp \--sport 901 -j ACCEPT
```

- string

​		字符串，对报文中的应用层数据做字符串模式匹配检测(通过算法实现)。

```bash
--algo {bm|kmp}：字符匹配查找时使用算法
--string "STRING": 要查找的字符串
--hex-string “HEX-STRING”: 要查找的字符，先编码成16进制格式
```

- connlimit

​		连接限制，根据每个客户端IP作并发连接数量限制。

```bash
--connlimit-upto n  #连接数小于等于n时匹配
--connlimit-above n #连接数大于n时匹配
```

- limit

​		报文速率限制

- state

​		追踪本机上的请求和响应之间的数据报文的状态。状态有五种：`INVALID`, `ESTABLISHED`,` NEW`, `RELATED`,`UNTRACKED`.

```bash
--state state
NEW         #新连接请求
ESTABLISHED #已建立的连接
INVALID 	#无法识别的连接
PELATED     #相关联的连接，当前连接是一个新请求，但附属于某个已存在的连接
UNTRACKED   #未追踪的连接
```



```
对于进入的状态为ESTABLISHED都应该放行；
对于出去的状态为ESTABLISHED都应该放行；
严格检查进入的状态为NEW的连接；
所有状态为INVALIED都应该拒绝；
```

## 11、iptables规则中动作

​		iptables规则中的动作常称为target，也分为基本动作和扩展动作。

- ACCEPT：允许数据包通过

- DROP：直接丢弃数据包，不给任何回应信息

- REJECT：拒绝数据包通过，发送回应信息给客户端

- SNAT：源地址转换

  - 解释1：数据包从网卡发送出去的时候，把数据包中的源地址部分替换为指定的IP，接收方认为数据包的来源是被替换的那个IP主机，返回响应时，也以被替换的IP地址进行。

  - 解释2：修改数据包源地址，当内网数据包到达防火墙后，防火墙会使用外部地址替换掉数据包的源IP地址（目的IP地址不变），使网络内部主机能够与网络外部主机通信。

![image-20230118120052457](.\image\image-20230118120052457.png)

```bash
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth1 -j SNAT --to-source 202.12.10.100
```

- MASQUERADE:伪装，类似于SNAT，适用于动态的、临时会变的ip地址上，例如：家庭使用的宽带。用发送数据的网卡上的IP来替换源IP，对于IP地址不固定场合使用。
- DNAT:目标地址转换
  - 解释1：数据包从网卡发出时，修改数据包中的目的IP，表现为你想访问A，但因网关做了DNAT，把所有访问A的数据包中的目的IP地址全部修改为B,实际最终访问的是B。
  - 解释2：改变数据包目的地址，当防火墙收到来自外网的数据包后，会将该数据包的目的IP地址进行替换（源IP地址不变），重新转发到内网的主机。
    

![image-20230118120359610](.\image\image-20230118120359610.png)

```bash
iptables -t nat -A PREROUTING -d 202.12.10.100 -p tcp --dport 80 -j DNAT --to-destination 192.168.10.1
```

- REDIRECT:在本机做端口映射
- LOG:在`/var/log/message`文件中记录日志信息，然后将数据包传递给下一条规则。

<blockquote>
    路由是按照目的地址进行路由选择的，因此，DNAT是在PREROUTING链上进行的，SNAT是在数据包发出的时候进行的，因此是在POSTROUTING链上进行的。
</blockquote>

## 12、制定iptables规则策略

- 黑名单

  没有被拒绝的流量都可以通过，这种策略下管理员必须针对每一种新出现的攻击，制定新的规则，因此不推荐

- 白名单

  没有被允许的流量都要拒绝，这种策略比较保守，根据需要，主机主机逐渐开放，目前一般都采用白名单策
  略，推荐。

## 13、制定iptables规则思路

​		1、选择一张表，此表决定了数据报文处理的方式

​		2、选择一条链，此链决定了数据报文流经哪些位置

​		3、选择合适的条件，此条件决定了对数据报文做何种条件匹配

​		4、选择处理数据报文的动作，制定相应的防火墙规则

## 14、iptables基础语法结构

```bash
iptables [-t 表名] 管理选项 [链名] [条件匹配] [-j 目标动作或跳转]
```

<blockquote>
    不指定表名时，默认表示ﬁlter表，不指定链名时，默认表示该表内所有链，除非设置规则链的默认策略，否则 需要指定匹配条件。
</blockquote>

<center><font color='red' size=5><b>iptable规则组成</b></font></center>
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
        <th rowspan='2'><center>iptables</center></th>
        <th><center>table</center></th><th><center>command</center></th><th><center>chain</center></th><th><center>Parameter&</br>Xmatch</center></th><th><center>target</center></th>
    </tr>
    <tr>
        <td>-t filter</br>nat</br>mangle</br>raw</td><td>-A</br>-D</br>-L</br>-P</br>-I</br>-R</br>-n</td><td>INPUT</br>FORWARD</br>OUTPUT</br>PREROUTING</br>POSROUTING</td><td>-p tcp</br>-s</br>-d</br>--sport</br>--dport</br>--dports</br>-m tcp</br>-m state</br>-m multiport</td><td>-j ACCEPT</br>DROP</br>REJECT</br>DNAT</br>SNAT</td>
    </tr>
</table>

## 15、iptables链管理方法

```bash
   -N, --new-chain chain：新建一个自定义的规则链；        
   -X, --delete-chain [chain]：删除用户自定义的引用计数为0的空链；        
   -F, --flush [chain]：清空指定的规则链上的规则；        
   -E, --rename-chain old-chain new-chain：重命名链；        
   -Z, --zero [chain [rulenum]]：置零计数器；          注意：每个规则都有两个计数器          packets：被本规则所匹配到的所有报文的个数；          bytes：被本规则所匹配到的所有报文的大小之和；        
   -P, --policy chain target 制定链表的策略(ACCEPT|DROP|REJECT)
```

## 16、iptables规则管理

```bash
      -A, --append chain rule-specification：追加新规则于指定链的尾部；         
      -I, --insert chain [rulenum] rule-specification：插入新规则于指定链的指定位置，默认为首部；        
      -R, --replace chain rulenum rule-specification：替换指定的规则为新的规则；        
      -D, --delete chain rulenum：根据规则编号删除规则；        
      -D, --delete chain rule-specification：根据规则本身删除规则；
```

## 17、iptables规则显示

```bash
         -L, --list [chain]  ：列出规则；           
         -v, --verbose：详细信息；            -vv 更详细的信息           
         -n, --numeric：数字格式显示主机地址和端口号；           
         -x, --exact：显示计数器的精确值，而非圆整后的数据；           --line-numbers：列出规则时，显示其在链上的相应的编号           
         -S, --list-rules [chain]：显示指定链的所有规则；
```



# 四、iptables应用

- 环境：<font size=5 color='red'><b>`openEuler-22.03-LTS`</b></font>

## 1、安装`iptables-services`

```bash
[root@openEuler ~]# yum -y install iptables-services
Last metadata expiration check: 1:53:18 ago on 2023年01月18日 星期三 10时34分04秒.
Package iptables-1.8.7-5.oe2203.x86_64 is already installed.
Dependencies resolved.
========================================================================================================================
 Package                        Architecture            Version                           Repository               Size
========================================================================================================================
Upgrading:
 iptables                       x86_64                  1.8.7-8.oe2203                    update                   76 k
 iptables-libs                  x86_64                  1.8.7-8.oe2203                    update                  249 k

Transaction Summary
========================================================================================================================
Upgrade  2 Packages

Total download size: 325 k
Downloading Packages:
(1/2): iptables-1.8.7-8.oe2203.x86_64.rpm                                               632 kB/s |  76 kB     00:00
(2/2): iptables-libs-1.8.7-8.oe2203.x86_64.rpm                                          1.4 MB/s | 249 kB     00:00
------------------------------------------------------------------------------------------------------------------------
Total                                                                                   1.8 MB/s | 325 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                1/1
  Running scriptlet: iptables-libs-1.8.7-8.oe2203.x86_64                                                            1/1
  Upgrading        : iptables-libs-1.8.7-8.oe2203.x86_64                                                            1/4
  Upgrading        : iptables-1.8.7-8.oe2203.x86_64                                                                 2/4
  Running scriptlet: iptables-1.8.7-8.oe2203.x86_64                                                                 2/4
  Running scriptlet: iptables-1.8.7-5.oe2203.x86_64                                                                 3/4
  Cleanup          : iptables-1.8.7-5.oe2203.x86_64                                                                 3/4
  Running scriptlet: iptables-1.8.7-5.oe2203.x86_64                                                                 3/4
  Cleanup          : iptables-libs-1.8.7-5.oe2203.x86_64                                                            4/4
  Running scriptlet: iptables-libs-1.8.7-5.oe2203.x86_64                                                            4/4
  Verifying        : iptables-1.8.7-8.oe2203.x86_64                                                                 1/4
  Verifying        : iptables-1.8.7-5.oe2203.x86_64                                                                 2/4
  Verifying        : iptables-libs-1.8.7-8.oe2203.x86_64                                                            3/4
  Verifying        : iptables-libs-1.8.7-5.oe2203.x86_64                                                            4/4

Upgraded:
  iptables-1.8.7-8.oe2203.x86_64                           iptables-libs-1.8.7-8.oe2203.x86_64

Complete!
[root@openEuler ~]#
```

- 设置服务开启

```bash
[root@openEuler ~]# systemctl start iptables
[root@openEuler ~]# systemctl status iptables
● iptables.service - IPv4 firewall with iptables
     Loaded: loaded (/usr/lib/systemd/system/iptables.service; disabled; vendor preset: disabled)
     Active: active (exited) since Wed 2023-01-18 13:31:25 CST; 9s ago
    Process: 5558 ExecStart=/usr/libexec/iptables/iptables.init start (code=exited, status=0/SUCCESS)
   Main PID: 5558 (code=exited, status=0/SUCCESS)

1月 18 13:31:25 openEuler systemd[1]: Starting IPv4 firewall with iptables...
1月 18 13:31:25 openEuler iptables.init[5558]: iptables: Applying firewall rules: [  OK  ]
1月 18 13:31:25 openEuler systemd[1]: Finished IPv4 firewall with iptables.
[root@openEuler ~]#
```

- 设置开机启动

```bash
[root@openEuler ~]# systemctl enable iptables
Created symlink /etc/systemd/system/basic.target.wants/iptables.service → /usr/lib/systemd/system/iptables.service.
[root@openEuler ~]#
```

- 查看配置文件

```bash
[root@openEuler2203 ~]# rpm -ql iptables
```





# 五、secGear

​		secGear是一款供开发者开发安全应用的机密计算框架

## 1、基本概念

### （1）概述

​		随着云计算的快速发展，越来越多的企业把计算业务部署到云上，对数据的保护变得更加复杂，同时，数据泄露是云计算面临的重大安全问题。因此，如何保障用户数据在云上的安全变得尤为重要。当前对数据的保护通常注重离线存储安全和网络传输安全，缺乏对数据运行时的安全防护。为了保障云端数据运行时的安全性，方便开发者开发云上应用，openEuler 推出了 secGear 。

​		secGear 是统一机密计算编程框架，提供了易用的开发套件，包括安全区（使用 secGear 编程会将系统区分为安全区域和非安全区域） 生命周期管理、安全开发库、代码辅助生成工具、代码构建与签名工具、安全能力和安全服务组件实现方案。可用于信任环、密态数据库、多方计算、AI安全保护等多种场景。

### （2）框架介绍

![image-20230118155248970](.\image\image-20230118155248970.png)

如图所示，secGear 主题包含三个层级（当前仅开源基础层 Base Layer，服务层和中间件层逐步开源）：

- 服务层：提供完整的运行在安全侧的安全服务。
- 中间件层：提供一套协议接口，满足用户基本安全应用。
- 基础层：提供丰富的 enclave 开发接口或工具，并且在安全侧支持 C POSIX APIs 和标准 OpenSSL 接口，用户基于这些接口可以自由开发安全应用程序 。

## 2、安装和使用

### （1）安装secGear

```bash
[root@openEuler2203 ~]# yum install -y secGear secGear-devel
Last metadata expiration check: 1:27:50 ago on 2023年01月18日 星期三 14时26分53秒.
Dependencies resolved.
========================================================================================================================
 Package                              Architecture      Version                             Repository             Size
========================================================================================================================
Installing:
 secGear                              x86_64            0.1.0-26.oe2203                     everything             30 k
 secGear-devel                        x86_64            0.1.0-26.oe2203                     everything            537 k
Installing group/module packages:
 kernel                               x86_64            5.10.0-60.63.0.89.oe2203            update                 55 M
Installing dependencies:
 cmake                                x86_64            3.22.0-4.oe2203                     OS                     11 M
 cmake-data                           noarch            3.22.0-4.oe2203                     OS                    1.7 M
 cmake-rpm-macros                     noarch            3.22.0-4.oe2203                     OS                     12 k
 jsoncpp                              x86_64            1.9.5-3.oe2203                      OS                     89 k
 libsgx-ae-le                         x86_64            2.15.1-8.oe2203                     update                143 k
 libsgx-aesm-launch-plugin            x86_64            2.15.1-8.oe2203                     update                 48 k
 libsgx-enclave-common                x86_64            2.15.1-8.oe2203                     update                 43 k
 libsgx-launch                        x86_64            2.15.1-8.oe2203                     update                 82 k
 libsgx-urts                          x86_64            2.15.1-8.oe2203                     update                 74 k
 linux-sgx-driver                     x86_64            2.11-6.oe2203                       update                 35 k
 sgx-aesm-service                     x86_64            2.15.1-8.oe2203                     update                564 k
 sgxsdk                               x86_64            2.15.1-8.oe2203                     update                7.5 M

Transaction Summary
========================================================================================================================
Install  15 Packages

Total download size: 76 M
Installed size: 169 M
Downloading Packages:
(1/15): cmake-rpm-macros-3.22.0-4.oe2203.noarch.rpm                                     212 kB/s |  12 kB     00:00
(2/15): jsoncpp-1.9.5-3.oe2203.x86_64.rpm                                               1.6 MB/s |  89 kB     00:00
(3/15): secGear-0.1.0-26.oe2203.x86_64.rpm                                              1.0 MB/s |  30 kB     00:00
(4/15): secGear-devel-0.1.0-26.oe2203.x86_64.rpm                                        2.0 MB/s | 537 kB     00:00
(5/15): cmake-data-3.22.0-4.oe2203.noarch.rpm                                           3.4 MB/s | 1.7 MB     00:00
(6/15): libsgx-ae-le-2.15.1-8.oe2203.x86_64.rpm                                         2.9 MB/s | 143 kB     00:00
(7/15): libsgx-aesm-launch-plugin-2.15.1-8.oe2203.x86_64.rpm                            2.0 MB/s |  48 kB     00:00
(8/15): libsgx-enclave-common-2.15.1-8.oe2203.x86_64.rpm                                1.6 MB/s |  43 kB     00:00
(9/15): libsgx-launch-2.15.1-8.oe2203.x86_64.rpm                                        1.9 MB/s |  82 kB     00:00
(10/15): libsgx-urts-2.15.1-8.oe2203.x86_64.rpm                                         2.7 MB/s |  74 kB     00:00
(11/15): linux-sgx-driver-2.11-6.oe2203.x86_64.rpm                                      1.6 MB/s |  35 kB     00:00
(12/15): sgx-aesm-service-2.15.1-8.oe2203.x86_64.rpm                                    3.3 MB/s | 564 kB     00:00
(13/15): sgxsdk-2.15.1-8.oe2203.x86_64.rpm                                              2.6 MB/s | 7.5 MB     00:02
(14/15): cmake-3.22.0-4.oe2203.x86_64.rpm                                               2.8 MB/s |  11 MB     00:03
(15/15): kernel-5.10.0-60.63.0.89.oe2203.x86_64.rpm                                     4.3 MB/s |  55 MB     00:12
------------------------------------------------------------------------------------------------------------------------
Total                                                                                   5.8 MB/s |  76 MB     00:13
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                1/1
  Installing       : sgxsdk-2.15.1-8.oe2203.x86_64                                                                 1/15
  Installing       : cmake-rpm-macros-3.22.0-4.oe2203.noarch                                                       2/15
  Installing       : sgx-aesm-service-2.15.1-8.oe2203.x86_64                                                       3/15
  Running scriptlet: sgx-aesm-service-2.15.1-8.oe2203.x86_64                                                       3/15
Created symlink /etc/systemd/system/multi-user.target.wants/aesmd.service → /usr/lib/systemd/system/aesmd.service.

  Installing       : libsgx-ae-le-2.15.1-8.oe2203.x86_64                                                           4/15
  Installing       : libsgx-aesm-launch-plugin-2.15.1-8.oe2203.x86_64                                              5/15
  Installing       : libsgx-launch-2.15.1-8.oe2203.x86_64                                                          6/15
  Installing       : libsgx-enclave-common-2.15.1-8.oe2203.x86_64                                                  7/15
  Running scriptlet: libsgx-enclave-common-2.15.1-8.oe2203.x86_64                                                  7/15
  Installing       : libsgx-urts-2.15.1-8.oe2203.x86_64                                                            8/15
  Installing       : kernel-5.10.0-60.63.0.89.oe2203.x86_64                                                        9/15
  Running scriptlet: kernel-5.10.0-60.63.0.89.oe2203.x86_64                                                        9/15
  Running scriptlet: linux-sgx-driver-2.11-6.oe2203.x86_64                                                        10/15
  Installing       : linux-sgx-driver-2.11-6.oe2203.x86_64                                                        10/15
  Running scriptlet: linux-sgx-driver-2.11-6.oe2203.x86_64                                                        10/15
cat: /etc/modules: 没有那个文件或目录
modprobe: FATAL: Module isgx not found in directory /lib/modules/5.10.0-60.18.0.50.oe2203.x86_64
warning: kernel has been updated, please reboot system with the latest version of kernel to enable sgx!

  Installing       : secGear-0.1.0-26.oe2203.x86_64                                                               11/15
  Running scriptlet: secGear-0.1.0-26.oe2203.x86_64                                                               11/15
  Installing       : jsoncpp-1.9.5-3.oe2203.x86_64                                                                12/15
  Running scriptlet: jsoncpp-1.9.5-3.oe2203.x86_64                                                                12/15
  Installing       : cmake-data-3.22.0-4.oe2203.noarch                                                            13/15
  Installing       : cmake-3.22.0-4.oe2203.x86_64                                                                 14/15
  Installing       : secGear-devel-0.1.0-26.oe2203.x86_64                                                         15/15
  Running scriptlet: kernel-5.10.0-60.63.0.89.oe2203.x86_64                                                       15/15
Error! echo
Your kernel headers for kernel 5.10.0-60.63.0.89.oe2203.x86_64 cannot be found at
/lib/modules/5.10.0-60.63.0.89.oe2203.x86_64/build or /lib/modules/5.10.0-60.63.0.89.oe2203.x86_64/source.
Error! echo
Your kernel headers for kernel 5.10.0-60.63.0.89.oe2203.x86_64 cannot be found at
/lib/modules/5.10.0-60.63.0.89.oe2203.x86_64/build or /lib/modules/5.10.0-60.63.0.89.oe2203.x86_64/source.

  Running scriptlet: secGear-devel-0.1.0-26.oe2203.x86_64                                                         15/15
  Verifying        : cmake-3.22.0-4.oe2203.x86_64                                                                  1/15
  Verifying        : cmake-data-3.22.0-4.oe2203.noarch                                                             2/15
  Verifying        : cmake-rpm-macros-3.22.0-4.oe2203.noarch                                                       3/15
  Verifying        : jsoncpp-1.9.5-3.oe2203.x86_64                                                                 4/15
  Verifying        : secGear-0.1.0-26.oe2203.x86_64                                                                5/15
  Verifying        : secGear-devel-0.1.0-26.oe2203.x86_64                                                          6/15
  Verifying        : kernel-5.10.0-60.63.0.89.oe2203.x86_64                                                        7/15
  Verifying        : libsgx-ae-le-2.15.1-8.oe2203.x86_64                                                           8/15
  Verifying        : libsgx-aesm-launch-plugin-2.15.1-8.oe2203.x86_64                                              9/15
  Verifying        : libsgx-enclave-common-2.15.1-8.oe2203.x86_64                                                 10/15
  Verifying        : libsgx-launch-2.15.1-8.oe2203.x86_64                                                         11/15
  Verifying        : libsgx-urts-2.15.1-8.oe2203.x86_64                                                           12/15
  Verifying        : linux-sgx-driver-2.11-6.oe2203.x86_64                                                        13/15
  Verifying        : sgx-aesm-service-2.15.1-8.oe2203.x86_64                                                      14/15
  Verifying        : sgxsdk-2.15.1-8.oe2203.x86_64                                                                15/15

Installed:
  cmake-3.22.0-4.oe2203.x86_64                                 cmake-data-3.22.0-4.oe2203.noarch
  cmake-rpm-macros-3.22.0-4.oe2203.noarch                      jsoncpp-1.9.5-3.oe2203.x86_64
  kernel-5.10.0-60.63.0.89.oe2203.x86_64                       libsgx-ae-le-2.15.1-8.oe2203.x86_64
  libsgx-aesm-launch-plugin-2.15.1-8.oe2203.x86_64             libsgx-enclave-common-2.15.1-8.oe2203.x86_64
  libsgx-launch-2.15.1-8.oe2203.x86_64                         libsgx-urts-2.15.1-8.oe2203.x86_64
  linux-sgx-driver-2.11-6.oe2203.x86_64                        secGear-0.1.0-26.oe2203.x86_64
  secGear-devel-0.1.0-26.oe2203.x86_64                         sgx-aesm-service-2.15.1-8.oe2203.x86_64
  sgxsdk-2.15.1-8.oe2203.x86_64

Complete!
[root@openEuler2203 ~]#
```

​		查看安装是否成功

```bash
[root@openEuler2203 ~]# rpm -q secGear ; rpm -q secGear-devel
secGear-0.1.0-26.oe2203.x86_64
secGear-devel-0.1.0-26.oe2203.x86_64
[root@openEuler2203 ~]#
```

### （2）使用secGear工具

​		secGear 提供了一套工具集，方便用户开发应用程序。

#### ① 代码生成工具 codegener

##### 简介

​		secGear codegener 是基于 intel SGX SDK edger8r 开发的工具，用于解析 EDL 文件生成中间 C 代码，即辅助生成安全测与非安全侧文件互相调用的代码。

- 只能在方法中使用 public，不加 public 的函数声明默认为 private
- 不支持从非安全侧到安全侧，以及安全侧到非安全侧的 Switchless Calls
- OCALL（Outside call) 不支持部分调用模式（如 cdecl，stdcall，fastcall）

​		EDL 文件语法为类 C 语言语法，这里主要描述与 C 语言的差异部分：

| 成员                | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| include "my_type.h" | 使用外部包含文件中定义的类型                                 |
| trusted             | 声明 TA（Trusted Application）侧可用安全函数                 |
| untrusted           | 声明 TA 侧可用不安全函数                                     |
| return_type         | 定义返回值类型                                               |
| parameter_type      | 定义参数类型                                                 |
| [in , size = len]   | 对ecall而言，表示该参数需要将数据从非安全侧传入安全侧，ocall反之（指针类型需要使用此参数，其中 size 表示实际使用的 buffer） |
| [out, size = len]   | 对ecall而言，表示该参数需要将数据从安全侧传出到非安全侧，ocall反之（指针类型需要使用此参数，其中 size 表示实际使用的 buffer） |

#####  使用说明

###### 命令格式

​		codegen 的命令格式如下：

- x86_64 架构：

```bash
codegen_x86_64 < --trustzone | --sgx > [--trusted-dir | --untrusted-dir | --trusted | --untrusted ] edlfile
```

- ARM 架构

```bash
codegen_arm64 < --trustzone | --sgx > [--trusted-dir | --untrusted-dir | --trusted | --untrusted ] edlfile
```

###### 命令格式

​		各参数含义如下：

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
        <th>参数</th>
        <th>是否可选</th>
        <th>参数含义</th>
    </tr>
    <tr>
        <td>--trustzone | --sgx</td>
        <td>必选</td>
        <td>只在当前运行命令目录下生成机密计算架构对应接口函数，不加参数默认生成 SGX 接口函数</td>
    </tr>
    <tr>
        <td>--search-patd</td>
        <td>可选</td>
        <td>用于指定被转译的edl文件所依赖文件的搜索路径</td>
    </tr>
    <tr>
        <td>--use-prefix</td>
        <td>可选</td>
        <td>用于给代理函数名称加上前缀，前缀名为edl的文件名</td>
    </tr>
    <tr>
        <td>--header-only</td>
        <td>可选</td>
        <td>指定代码生成工具只生成头文件</td>
    </tr>
    <tr>
        <td>--trusted-dir</td>
        <td>可选</td>
        <td>指定生成安全侧辅助代码所在目录，不指定该参数默认为当前路径</td>
    </tr>
    <tr>
        <td>--untrusted-dir</td>
        <td>可选</td>
        <td>指定生成非安全侧函数辅助代码所在目录</td>
    </tr>
    <tr>
        <td>--trusted</td>
        <td>可选</td>
        <td>生成安全侧辅助代码</td>
    </tr>
    <tr>
        <td>--untrusted</td>
        <td>可选</td>
        <td>生成非安全侧辅助代码</td>
    </tr>
    <tr>
        <td>edlfile</td>
        <td>必选</td>
        <td>需要转译的 EDL 文件，例如 hello.edl</td>
    </tr>
</table>
