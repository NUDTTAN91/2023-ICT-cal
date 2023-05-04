- 环境：<font size=5 color='red'><b>`openEuler-22.03-LTS`</b></font>

# 一、iptables-service安装

​		我们本地的iptables是存在的

```bash
[root@openEuler2203 ~]# iptables
iptables v1.8.7 (legacy): no command specified
Try `iptables -h' or 'iptables --help' for more information.
[root@openEuler2203 ~]#
```

​		发现它会告诉你iptables使用的是v1.8.7版本的，但是没有命令被提供，`-t`选择一张表，`-L`是查看它所有的规则，

```bash
[root@openEuler2203 ~]# iptables -t filter -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
LIBVIRT_INP  all  --  anywhere             anywhere

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
LIBVIRT_FWX  all  --  anywhere             anywhere
LIBVIRT_FWI  all  --  anywhere             anywhere
LIBVIRT_FWO  all  --  anywhere             anywhere

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
LIBVIRT_OUT  all  --  anywhere             anywhere

Chain LIBVIRT_FWI (1 references)
target     prot opt source               destination
ACCEPT     all  --  anywhere             192.168.122.0/24     ctstate RELATED,ESTABLISHED
REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

Chain LIBVIRT_FWO (1 references)
target     prot opt source               destination
ACCEPT     all  --  192.168.122.0/24     anywhere
REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

Chain LIBVIRT_FWX (1 references)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere

Chain LIBVIRT_INP (1 references)
target     prot opt source               destination
ACCEPT     udp  --  anywhere             anywhere             udp dpt:domain
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootps
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:bootps

Chain LIBVIRT_OUT (1 references)
target     prot opt source               destination
ACCEPT     udp  --  anywhere             anywhere             udp dpt:domain
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootpc
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:bootpc
[root@openEuler2203 ~]#
```



![image-20230220160018893](D:\Users\Administrator\Desktop\【01】Folder\【openEuler国赛培训】珠海城市职业技术学院\【07】\image\image-20230220160018893.png)