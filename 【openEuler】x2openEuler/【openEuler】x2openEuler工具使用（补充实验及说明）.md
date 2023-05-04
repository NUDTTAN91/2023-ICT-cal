<center><font size=7><b>【openEuler】x2openEuler工具使用</b></font></center>

<center><font size=6><b>补充实验及说明</b></font></center>

+++

[toc]

+++



### 1、连通性测试

​		这个测试是测试两个方面，第一个就是两台机的连通性，第二个就是本机和源的连通性。

### 2、升级前检查`任务检查中断，中断原因是：软件冲突检查执行失败`

![image-20230303090516179](.\image\image-20230303090516179.png)

​		查看日志发现错误：

```bash
2023-03-03 09:04:27,906 - Client_IP:UNKNOWN - USER_ID:1001 - UUID:4a579caa-b95f-11ed-81ad-000c295e45f4 - ERROR - Abnormal exit, error: /etc/x2openEuler/database_2.0.0.630/centos7.9/x86_64/primary not exists, please check whether the database is imported
```

![image-20230303090628842](.\image\image-20230303090628842.png)

​		因缺少database对应的版本目录，手动创建目录，具体操作命令如下：

```bash
[root@localhost ~]# cd /etc/x2openEuler/database_2.0.0.630/
[root@localhost database_2.0.0.630]# cp -r centos7.6 centos7.9
[root@localhost database_2.0.0.630]# cp -r centos7.6-openEuler20.03-LTS-SP1 centos7.9-openEuler20.03-LTS-SP1
[root@localhost database_2.0.0.630]# chown -R x2openEuler.x2openEuler centos7.9
[root@localhost database_2.0.0.630]# chown -R x2openEuler.x2openEuler centos7.9-openEuler20.03-LTS-SP1/
[root@localhost database_2.0.0.630]# cd /etc/x2openEuler/database_2.0.0.630/centos7.9
[root@localhost centos7.9]# mv centos7.6.json centos7.9.json
[root@localhost centos7.9]# cd /etc/x2openEuler/database_2.0.0.630/centos7.9-openEuler20.03-LTS-SP1
[root@localhost centos7.9-openEuler20.03-LTS-SP1]# mv centos7.6-openEuler20.03-LTS-SP1.json centos7.9-openEuler20.03-LTS-SP1.json
[root@localhost centos7.9-openEuler20.03-LTS-SP1]# cd /etc/x2openEuler/database_2.0.0.630/centos7.9-openEuler20.03-LTS-SP1/x86_64
[root@localhost x86_64]# mv centos7.6.sqlite centos7.9.sqlite
[root@localhost x86_64]# mv centos7.6-openEuler20.03-LTS-SP1.sqlite centos7.9-openEuler20.03-LTS-SP1.sqlite
[root@localhost x86_64]#
```

### 3、升级

​		升级的过程中网速会打满，一定要保持网络连接稳定

![image-20230303093340753](.\image\image-20230303093340753.png)

![image-20230303093443406](.\image\image-20230303093443406.png)



## 使用本地镜像源升级

### 1、将`openEuler-22.03-LTS-SP1-everything-x86_64-dvd.iso`解压后拖入hfs

![image-20230306094106066](.\image\image-20230306094106066.png)

### 2、配置本地`repo`

<font size=4 color='red'><b>`migrate-local.repo`</b></font>

```bash
[local]
name=local
baseurl=http://xxx.xxx.xxx.xxx/openEuler-22.03-LTS-SP1-everything-x86_64-dvd/
enable=1
gpgcheck=0
```

![image-20230306094336977](.\image\image-20230306094336977.png)

​		然后就可以开始了