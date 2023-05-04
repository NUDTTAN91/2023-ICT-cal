<center><font size=7><b>openEuler智能调优</b></font></center>

+++

[TOC]

+++

# 一、环境准备

- openEuler-20.03-LTS

# 二、安装过程

## 1、安装依赖软件包

```bash
[root@openEuler ~]# yum install -y golang-bin python3 perf sysstat hwloc-gui
Last metadata expiration check: 0:01:01 ago on 2023年01月13日 星期五 11时31分38秒.
Package python3-3.7.4-8.oe1.x86_64 is already installed.
Package perf-4.19.90-2003.4.0.0036.oe1.x86_64 is already installed.
Package sysstat-12.1.6-2.oe1.x86_64 is already installed.
Dependencies resolved.
=============================================================================================================================
 Package                          Architecture               Version                            Repository              Size
=============================================================================================================================
Installing:
 golang                           x86_64                     1.13-3.3.oe1                       OS                     112 M
 hwloc                            x86_64                     1.11.9-3.oe1                       OS                     1.7 M
Installing dependencies:
 golang-devel                     noarch                     1.13-3.3.oe1                       OS                      14 M

Transaction Summary
=============================================================================================================================
Install  3 Packages

Total download size: 128 M
Installed size: 439 M
Downloading Packages:
(1/3): hwloc-1.11.9-3.oe1.x86_64.rpm                                                         557 kB/s | 1.7 MB     00:03    
(2/3): golang-devel-1.13-3.3.oe1.noarch.rpm                                                  1.9 MB/s |  14 MB     00:07    
(3/3): golang-1.13-3.3.oe1.x86_64.rpm                                                        4.3 MB/s | 112 MB     00:26    
-----------------------------------------------------------------------------------------------------------------------------
Total                                                                                        4.9 MB/s | 128 MB     00:26     
警告：/var/cache/dnf/OS-4d13fabbb2aeb21e/packages/golang-1.13-3.3.oe1.x86_64.rpm: 头V3 RSA/SHA1 Signature, 密钥 ID b25e7f66: NOKEY
OS                                                                                           133  B/s | 2.1 kB     00:16    
Importing GPG key 0xB25E7F66:
 Userid     : "private OBS (key without passphrase) <defaultkey@localobs>"
 Fingerprint: 12EA 74AC 9DF4 8D46 C69C A0BE D557 065E B25E 7F66
 From       : http://repo.openeuler.org/openEuler-20.03-LTS/OS/x86_64/RPM-GPG-KEY-openEuler
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: golang-1.13-3.3.oe1.x86_64                                                                          1/1 
  Preparing        :                                                                                                     1/1 
  Installing       : golang-devel-1.13-3.3.oe1.noarch                                                                    1/3 
  Installing       : golang-1.13-3.3.oe1.x86_64                                                                          2/3 
  Running scriptlet: golang-1.13-3.3.oe1.x86_64                                                                          2/3 
  Installing       : hwloc-1.11.9-3.oe1.x86_64                                                                           3/3 
  Running scriptlet: hwloc-1.11.9-3.oe1.x86_64                                                                           3/3 
  Verifying        : golang-1.13-3.3.oe1.x86_64                                                                          1/3 
  Verifying        : golang-devel-1.13-3.3.oe1.noarch                                                                    2/3 
  Verifying        : hwloc-1.11.9-3.oe1.x86_64                                                                           3/3 

Installed:
  golang-1.13-3.3.oe1.x86_64             hwloc-1.11.9-3.oe1.x86_64             golang-devel-1.13-3.3.oe1.noarch            

Complete!
[root@openEuler ~]#
```



## 2、安装A-Tune服务的依赖包

```bash
[root@openEuler ~]# yum install -y python3-dict2xml python3-flask-restful python3-pandas python3-scikit-optimize python3-xgboost python3-pyyaml
Last metadata expiration check: 0:02:37 ago on 2023年01月13日 星期五 11时31分38秒.
Package python3-pyyaml-5.1.2-1.oe1.x86_64 is already installed.
Dependencies resolved.
=============================================================================================================================
 Package                                  Architecture            Version                          Repository           Size
=============================================================================================================================
Installing:
 python3-dict2xml                         noarch                  1.6.1-1.oe1                      OS                   18 k
 python3-flask-restful                    noarch                  0.3.6-10.oe1                     OS                  121 k
 python3-pandas                           x86_64                  0.25.3-1.oe1                     OS                   10 M
 python3-scikit-optimize                  noarch                  0.5.2-1.oe1                      OS                  104 k
 python3-xgboost                          x86_64                  0.90-2.oe1                       OS                  4.7 M
Installing dependencies:
 openblas                                 x86_64                  0.3.3-3.oe1                      OS                   99 M
 openblas-devel                           x86_64                  0.3.3-3.oe1                      OS                   83 k
 python3-aniso8601                        noarch                  7.0.0-1.oe1                      OS                   74 k
 python3-babel                            noarch                  2.7.0-1.oe1                      OS                  5.9 M
 python3-click                            noarch                  7.0-1.oe1                        OS                  145 k
 python3-flask                            noarch                  1:1.0.4-3.oe1                    OS                  155 k
 python3-itsdangerous                     noarch                  1.1.0-1.oe1                      OS                   36 k
 python3-jinja2                           noarch                  2.10-10.oe1                      OS                  222 k
 python3-joblib                           noarch                  0.14.0-3.oe1                     OS                  446 k
 python3-markupsafe                       x86_64                  1.0-3.oe1                        OS                   28 k
 python3-numpy                            x86_64                  1:1.16.5-2.oe1                   OS                  4.0 M
 python3-pytz                             noarch                  2019.2-2.oe1                     OS                   48 k
 python3-scikit-learn                     x86_64                  0.20.4-2.oe1                     OS                   11 M
 python3-scipy                            x86_64                  1.2.2-2.oe1                      OS                   14 M
 python3-werkzeug                         x86_64                  0.14.1-6.oe1                     OS                  465 k

Transaction Summary
=============================================================================================================================
Install  20 Packages

Total download size: 151 M
Installed size: 1.1 G
Downloading Packages:
(1/20): python3-aniso8601-7.0.0-1.oe1.noarch.rpm                                              15 kB/s |  74 kB     00:05    
(2/20): openblas-devel-0.3.3-3.oe1.x86_64.rpm                                                 16 kB/s |  83 kB     00:05    
(3/20): python3-click-7.0-1.oe1.noarch.rpm                                                   1.6 MB/s | 145 kB     00:00    
(4/20): python3-dict2xml-1.6.1-1.oe1.noarch.rpm                                              897 kB/s |  18 kB     00:00    
(5/20): python3-flask-1.0.4-3.oe1.noarch.rpm                                                 2.7 MB/s | 155 kB     00:00    
(6/20): python3-flask-restful-0.3.6-10.oe1.noarch.rpm                                        1.9 MB/s | 121 kB     00:00    
(7/20): python3-itsdangerous-1.1.0-1.oe1.noarch.rpm                                          1.0 MB/s |  36 kB     00:00    
(8/20): python3-jinja2-2.10-10.oe1.noarch.rpm                                                1.6 MB/s | 222 kB     00:00    
(9/20): python3-joblib-0.14.0-3.oe1.noarch.rpm                                               2.6 MB/s | 446 kB     00:00    
(10/20): python3-markupsafe-1.0-3.oe1.x86_64.rpm                                             979 kB/s |  28 kB     00:00    
(11/20): python3-numpy-1.16.5-2.oe1.x86_64.rpm                                               2.6 MB/s | 4.0 MB     00:01    
(12/20): python3-babel-2.7.0-1.oe1.noarch.rpm                                                1.9 MB/s | 5.9 MB     00:03    
(13/20): python3-pytz-2019.2-2.oe1.noarch.rpm                                                2.7 MB/s |  48 kB     00:00    
(14/20): python3-pandas-0.25.3-1.oe1.x86_64.rpm                                              2.5 MB/s |  10 MB     00:04    
(15/20): python3-scikit-optimize-0.5.2-1.oe1.noarch.rpm                                      3.0 MB/s | 104 kB     00:00    
(16/20): python3-scikit-learn-0.20.4-2.oe1.x86_64.rpm                                        1.8 MB/s |  11 MB     00:06    
(17/20): python3-werkzeug-0.14.1-6.oe1.x86_64.rpm                                            2.0 MB/s | 465 kB     00:00    
(18/20): python3-xgboost-0.90-2.oe1.x86_64.rpm                                               2.2 MB/s | 4.7 MB     00:02    
(19/20): python3-scipy-1.2.2-2.oe1.x86_64.rpm                                                2.4 MB/s |  14 MB     00:06    
(20/20): openblas-0.3.3-3.oe1.x86_64.rpm                                                     2.5 MB/s |  99 MB     00:39    
-----------------------------------------------------------------------------------------------------------------------------
Total                                                                                        3.8 MB/s | 151 MB     00:39     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                     1/1 
  Installing       : openblas-devel-0.3.3-3.oe1.x86_64                                                                  1/20 
  Installing       : openblas-0.3.3-3.oe1.x86_64                                                                        2/20 
  Running scriptlet: openblas-0.3.3-3.oe1.x86_64                                                                        2/20 
  Installing       : python3-numpy-1:1.16.5-2.oe1.x86_64                                                                3/20 
  Installing       : python3-scipy-1.2.2-2.oe1.x86_64                                                                   4/20 
  Installing       : python3-pytz-2019.2-2.oe1.noarch                                                                   5/20 
  Installing       : python3-babel-2.7.0-1.oe1.noarch                                                                   6/20 
  Installing       : python3-werkzeug-0.14.1-6.oe1.x86_64                                                               7/20 
  Installing       : python3-markupsafe-1.0-3.oe1.x86_64                                                                8/20 
  Installing       : python3-jinja2-2.10-10.oe1.noarch                                                                  9/20 
  Installing       : python3-joblib-0.14.0-3.oe1.noarch                                                                10/20 
  Installing       : python3-scikit-learn-0.20.4-2.oe1.x86_64                                                          11/20 
  Installing       : python3-itsdangerous-1.1.0-1.oe1.noarch                                                           12/20 
  Installing       : python3-click-7.0-1.oe1.noarch                                                                    13/20 
  Installing       : python3-flask-1:1.0.4-3.oe1.noarch                                                                14/20 
  Installing       : python3-aniso8601-7.0.0-1.oe1.noarch                                                              15/20 
  Installing       : python3-flask-restful-0.3.6-10.oe1.noarch                                                         16/20 
  Installing       : python3-scikit-optimize-0.5.2-1.oe1.noarch                                                        17/20 
  Installing       : python3-pandas-0.25.3-1.oe1.x86_64                                                                18/20 
  Installing       : python3-xgboost-0.90-2.oe1.x86_64                                                                 19/20 
  Installing       : python3-dict2xml-1.6.1-1.oe1.noarch                                                               20/20 
  Running scriptlet: python3-dict2xml-1.6.1-1.oe1.noarch                                                               20/20 
  Verifying        : openblas-0.3.3-3.oe1.x86_64                                                                        1/20 
  Verifying        : openblas-devel-0.3.3-3.oe1.x86_64                                                                  2/20 
  Verifying        : python3-aniso8601-7.0.0-1.oe1.noarch                                                               3/20 
  Verifying        : python3-babel-2.7.0-1.oe1.noarch                                                                   4/20 
  Verifying        : python3-click-7.0-1.oe1.noarch                                                                     5/20 
  Verifying        : python3-dict2xml-1.6.1-1.oe1.noarch                                                                6/20 
  Verifying        : python3-flask-1:1.0.4-3.oe1.noarch                                                                 7/20 
  Verifying        : python3-flask-restful-0.3.6-10.oe1.noarch                                                          8/20 
  Verifying        : python3-itsdangerous-1.1.0-1.oe1.noarch                                                            9/20 
  Verifying        : python3-jinja2-2.10-10.oe1.noarch                                                                 10/20 
  Verifying        : python3-joblib-0.14.0-3.oe1.noarch                                                                11/20 
  Verifying        : python3-markupsafe-1.0-3.oe1.x86_64                                                               12/20 
  Verifying        : python3-numpy-1:1.16.5-2.oe1.x86_64                                                               13/20 
  Verifying        : python3-pandas-0.25.3-1.oe1.x86_64                                                                14/20 
  Verifying        : python3-pytz-2019.2-2.oe1.noarch                                                                  15/20 
  Verifying        : python3-scikit-learn-0.20.4-2.oe1.x86_64                                                          16/20 
  Verifying        : python3-scikit-optimize-0.5.2-1.oe1.noarch                                                        17/20 
  Verifying        : python3-scipy-1.2.2-2.oe1.x86_64                                                                  18/20 
  Verifying        : python3-werkzeug-0.14.1-6.oe1.x86_64                                                              19/20 
  Verifying        : python3-xgboost-0.90-2.oe1.x86_64                                                                 20/20 

Installed:
  python3-dict2xml-1.6.1-1.oe1.noarch                       python3-flask-restful-0.3.6-10.oe1.noarch                       
  python3-pandas-0.25.3-1.oe1.x86_64                        python3-scikit-optimize-0.5.2-1.oe1.noarch                      
  python3-xgboost-0.90-2.oe1.x86_64                         openblas-0.3.3-3.oe1.x86_64                                     
  openblas-devel-0.3.3-3.oe1.x86_64                         python3-aniso8601-7.0.0-1.oe1.noarch                            
  python3-babel-2.7.0-1.oe1.noarch                          python3-click-7.0-1.oe1.noarch                                  
  python3-flask-1:1.0.4-3.oe1.noarch                        python3-itsdangerous-1.1.0-1.oe1.noarch                         
  python3-jinja2-2.10-10.oe1.noarch                         python3-joblib-0.14.0-3.oe1.noarch                              
  python3-markupsafe-1.0-3.oe1.x86_64                       python3-numpy-1:1.16.5-2.oe1.x86_64                             
  python3-pytz-2019.2-2.oe1.noarch                          python3-scikit-learn-0.20.4-2.oe1.x86_64                        
  python3-scipy-1.2.2-2.oe1.x86_64                          python3-werkzeug-0.14.1-6.oe1.x86_64                            

Complete!
[root@openEuler ~]#
```



## 3、安装数据库依赖包

```bash
[root@openEuler ~]# yum install -y python3-sqlalchemy python3-cryptography
Last metadata expiration check: 0:04:30 ago on 2023年01月13日 星期五 11时31分38秒.
Dependencies resolved.
=============================================================================================================================
 Package                              Architecture           Version                        Repository                  Size
=============================================================================================================================
Installing:
 python3-cryptography                 x86_64                 2.6.1-1.oe1                    OS                         390 k
 python3-sqlalchemy                   x86_64                 1.2.11-2.oe1                   everything                 1.8 M
Installing dependencies:
 python3-asn1crypto                   noarch                 0.24.0-8.oe1                   OS                         180 k
 python3-cffi                         x86_64                 1.11.5-10.oe1                  OS                         227 k
 python3-pycparser                    noarch                 2.19-1.oe1                     OS                         150 k

Transaction Summary
=============================================================================================================================
Install  5 Packages

Total download size: 2.7 M
Installed size: 14 M
Downloading Packages:
(1/5): python3-asn1crypto-0.24.0-8.oe1.noarch.rpm                                            1.0 MB/s | 180 kB     00:00    
(2/5): python3-pycparser-2.19-1.oe1.noarch.rpm                                               2.0 MB/s | 150 kB     00:00    
(3/5): python3-cffi-1.11.5-10.oe1.x86_64.rpm                                                 518 kB/s | 227 kB     00:00    
(4/5): python3-cryptography-2.6.1-1.oe1.x86_64.rpm                                           340 kB/s | 390 kB     00:01    
(5/5): python3-sqlalchemy-1.2.11-2.oe1.x86_64.rpm                                            1.1 MB/s | 1.8 MB     00:01    
-----------------------------------------------------------------------------------------------------------------------------
Total                                                                                        1.4 MB/s | 2.7 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                     1/1 
  Installing       : python3-pycparser-2.19-1.oe1.noarch                                                                 1/5 
  Installing       : python3-cffi-1.11.5-10.oe1.x86_64                                                                   2/5 
  Installing       : python3-asn1crypto-0.24.0-8.oe1.noarch                                                              3/5 
  Installing       : python3-cryptography-2.6.1-1.oe1.x86_64                                                             4/5 
  Installing       : python3-sqlalchemy-1.2.11-2.oe1.x86_64                                                              5/5 
  Running scriptlet: python3-sqlalchemy-1.2.11-2.oe1.x86_64                                                              5/5 
  Verifying        : python3-asn1crypto-0.24.0-8.oe1.noarch                                                              1/5 
  Verifying        : python3-cffi-1.11.5-10.oe1.x86_64                                                                   2/5 
  Verifying        : python3-cryptography-2.6.1-1.oe1.x86_64                                                             3/5 
  Verifying        : python3-pycparser-2.19-1.oe1.noarch                                                                 4/5 
  Verifying        : python3-sqlalchemy-1.2.11-2.oe1.x86_64                                                              5/5 

Installed:
  python3-cryptography-2.6.1-1.oe1.x86_64   python3-sqlalchemy-1.2.11-2.oe1.x86_64   python3-asn1crypto-0.24.0-8.oe1.noarch  
  python3-cffi-1.11.5-10.oe1.x86_64         python3-pycparser-2.19-1.oe1.noarch     

Complete!
[root@openEuler ~]#
```



```bash
[root@openEuler ~]# yum install -y python3-psycopg2
Last metadata expiration check: 0:04:58 ago on 2023年01月13日 星期五 11时31分38秒.
Dependencies resolved.
=============================================================================================================================
 Package                           Architecture            Version                         Repository                   Size
=============================================================================================================================
Installing:
 python3-psycopg2                  x86_64                  2.7.5-3.2.oe1                   everything                  154 k
Installing dependencies:
 postgresql-libs                   x86_64                  10.5-12.oe1                     OS                          249 k

Transaction Summary
=============================================================================================================================
Install  2 Packages

Total download size: 403 k
Installed size: 1.4 M
Downloading Packages:
(1/2): python3-psycopg2-2.7.5-3.2.oe1.x86_64.rpm                                             919 kB/s | 154 kB     00:00    
(2/2): postgresql-libs-10.5-12.oe1.x86_64.rpm                                                1.2 MB/s | 249 kB     00:00    
-----------------------------------------------------------------------------------------------------------------------------
Total                                                                                        2.0 MB/s | 403 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                     1/1 
  Installing       : postgresql-libs-10.5-12.oe1.x86_64                                                                  1/2 
  Running scriptlet: postgresql-libs-10.5-12.oe1.x86_64                                                                  1/2 
  Installing       : python3-psycopg2-2.7.5-3.2.oe1.x86_64                                                               2/2 
  Running scriptlet: python3-psycopg2-2.7.5-3.2.oe1.x86_64                                                               2/2 
  Verifying        : postgresql-libs-10.5-12.oe1.x86_64                                                                  1/2 
  Verifying        : python3-psycopg2-2.7.5-3.2.oe1.x86_64                                                               2/2 

Installed:
  python3-psycopg2-2.7.5-3.2.oe1.x86_64                          postgresql-libs-10.5-12.oe1.x86_64                         

Complete!
[root@openEuler ~]#
```



## 4、下载源码并编译安装

```bash
[root@openEuler ~]# git clone https://gitee.com/openeuler/A-Tune.git
正克隆到 'A-Tune'...
remote: Enumerating objects: 9034, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 9034 (delta 0), reused 0 (delta 0), pack-reused 9033
接收对象中: 100% (9034/9034), 109.39 MiB | 2.90 MiB/s, 完成.
处理 delta 中: 100% (4794/4794), 完成.
[root@openEuler ~]# ll
总用量 8.0K
-rw-------.  1 root root 1.4K  1月 13  2023 anaconda-ks.cfg
drwx------. 22 root root 4.0K  1月 13 11:40 A-Tune
[root@openEuler ~]# cd A-Tune
```



```bash
[root@openEuler A-Tune]# make models
cd /root/A-Tune/tools/ && python3 generate_models.py --model_path /usr/libexec/atuned/analysis/models 
Traceback (most recent call last):
  File "generate_models.py", line 54, in <module>
    main(ARGS.csv_path, ARGS.model_path, ARGS.select, ARGS.search)
  File "generate_models.py", line 38, in main
    processor.train(csv_path, feature_selection, search)
  File "/root/A-Tune/tools/../analysis/optimizer/workload_characterization.py", line 208, in train
    joblib.dump(self.tencoder, tencoder_path)
  File "/usr/lib/python3.7/site-packages/joblib/numpy_pickle.py", line 504, in dump
    with open(filename, 'wb') as f:
FileNotFoundError: [Errno 2] No such file or directory: '/usr/libexec/atuned/analysis/models/tencoder.pkl'
make: *** [Makefile:124：models] 错误 1
[root@openEuler A-Tune]#
```

​		**`错误`可以直接忽略，然后进行下一步。**



```bash
[root@openEuler A-Tune]# make
cd /root/A-Tune/modules/server/profile/ && go build -mod=vendor -v -ldflags '-s -w -extldflags "-static" -extldflags "-zrelro" -extldflags "-znow" -extldflags "-ftrapv" -X gitee.com/openeuler/A-Tune/common/config.Version=1.0.0(5735242)' -buildmode=plugin -o /root/A-Tune/pkg/daemon_profile_server.so *.go
github.com/golang-collections/collections/stack
github.com/golang/protobuf/proto
golang.org/x/net/internal/timeseries
golang.org/x/net/trace
google.golang.org/grpc/grpclog
google.golang.org/grpc/connectivity
google.golang.org/grpc/credentials/internal
google.golang.org/grpc/internal
google.golang.org/grpc/metadata
google.golang.org/grpc/serviceconfig
google.golang.org/grpc/resolver
google.golang.org/grpc/internal/grpcrand
google.golang.org/grpc/codes
google.golang.org/grpc/encoding
google.golang.org/grpc/internal/backoff
google.golang.org/grpc/internal/balancerload
golang.org/x/sys/unix
google.golang.org/grpc/credentials
google.golang.org/grpc/encoding/proto
google.golang.org/grpc/balancer
github.com/golang/protobuf/ptypes/any
google.golang.org/grpc/balancer/base
github.com/golang/protobuf/ptypes/duration
github.com/golang/protobuf/ptypes/timestamp
google.golang.org/grpc/balancer/roundrobin
github.com/golang/protobuf/ptypes
google.golang.org/grpc/binarylog/grpc_binarylog_v1
google.golang.org/genproto/googleapis/rpc/status
google.golang.org/grpc/status
google.golang.org/grpc/internal/channelz
google.golang.org/grpc/internal/binarylog
google.golang.org/grpc/internal/envconfig
google.golang.org/grpc/internal/grpcsync
golang.org/x/text/transform
golang.org/x/text/unicode/bidi
golang.org/x/text/unicode/norm
golang.org/x/text/secure/bidirule
golang.org/x/net/http2/hpack
google.golang.org/grpc/internal/syscall
google.golang.org/grpc/keepalive
google.golang.org/grpc/peer
google.golang.org/grpc/stats
google.golang.org/grpc/tap
google.golang.org/grpc/naming
golang.org/x/net/idna
google.golang.org/grpc/resolver/dns
google.golang.org/grpc/resolver/passthrough
github.com/go-ini/ini
golang.org/x/net/http/httpguts
golang.org/x/net/http2
github.com/sirupsen/logrus
github.com/sirupsen/logrus/hooks/syslog
gitee.com/openeuler/A-Tune/common/log
github.com/urfave/cli
google.golang.org/grpc/internal/transport
gopkg.in/yaml.v2
google.golang.org/grpc
gitee.com/openeuler/A-Tune/common/sched
xorm.io/builder
xorm.io/core
gitee.com/openeuler/A-Tune/api/profile
github.com/go-xorm/xorm
gitee.com/openeuler/A-Tune/common/utils
gitee.com/openeuler/A-Tune/common/config
gitee.com/openeuler/A-Tune/common/http
gitee.com/openeuler/A-Tune/common/models
gitee.com/openeuler/A-Tune/common/registry
gitee.com/openeuler/A-Tune/common/system
gitee.com/openeuler/A-Tune/common/checker
gitee.com/openeuler/A-Tune/common/schedule/filters
gitee.com/openeuler/A-Tune/common/cpumask
gitee.com/openeuler/A-Tune/common/topology
gitee.com/openeuler/A-Tune/common/schedule/framework
gitee.com/openeuler/A-Tune/common/sysload
gitee.com/openeuler/A-Tune/common/schedule/plugins/powersave
github.com/mattn/go-sqlite3
gitee.com/openeuler/A-Tune/common/schedule/plugins/numaaware
gitee.com/openeuler/A-Tune/common/schedule/plugins
gitee.com/openeuler/A-Tune/common/service
gitee.com/openeuler/A-Tune/common/client
gitee.com/openeuler/A-Tune/common/project
github.com/antlr/antlr4/runtime/Go/antlr
github.com/caibirdme/yql/internal/grammar
github.com/caibirdme/yql/internal/stack
github.com/caibirdme/yql
github.com/mitchellh/mapstructure
github.com/juju/errors
github.com/newm4n/grool/antlr/parser
github.com/newm4n/grool/pkg
github.com/newm4n/grool/context
github.com/newm4n/grool/model
github.com/newm4n/grool/antlr
github.com/newm4n/grool/builder
github.com/newm4n/grool/engine
gitee.com/openeuler/A-Tune/common/sqlstore
gitee.com/openeuler/A-Tune/common/schedule
gitee.com/openeuler/A-Tune/common/profile
gitee.com/openeuler/A-Tune/common/tuning
command-line-arguments
go build -mod=vendor -v -ldflags '-s -w -extldflags "-static" -extldflags "-zrelro" -extldflags "-znow" -extldflags "-ftrapv" -X gitee.com/openeuler/A-Tune/common/config.Version=1.0.0(5735242)' -o pkg/atune-adm cmd/atune-adm/*.go
golang.org/x/sys/unix
github.com/go-ini/ini
github.com/golang/protobuf/proto
github.com/sirupsen/logrus
github.com/sirupsen/logrus/hooks/syslog
gitee.com/openeuler/A-Tune/common/log
golang.org/x/net/internal/timeseries
golang.org/x/net/trace
google.golang.org/grpc/grpclog
google.golang.org/grpc/connectivity
google.golang.org/grpc/credentials/internal
google.golang.org/grpc/credentials
google.golang.org/grpc/internal
google.golang.org/grpc/metadata
google.golang.org/grpc/serviceconfig
google.golang.org/grpc/internal/grpcrand
google.golang.org/grpc/resolver
google.golang.org/grpc/codes
google.golang.org/grpc/encoding
google.golang.org/grpc/balancer
google.golang.org/grpc/encoding/proto
google.golang.org/grpc/balancer/base
google.golang.org/grpc/internal/backoff
google.golang.org/grpc/balancer/roundrobin
google.golang.org/grpc/internal/balancerload
github.com/golang/protobuf/ptypes/any
github.com/golang/protobuf/ptypes/duration
github.com/golang/protobuf/ptypes/timestamp
google.golang.org/genproto/googleapis/rpc/status
github.com/golang/protobuf/ptypes
google.golang.org/grpc/binarylog/grpc_binarylog_v1
google.golang.org/grpc/status
google.golang.org/grpc/internal/channelz
google.golang.org/grpc/internal/binarylog
google.golang.org/grpc/internal/envconfig
google.golang.org/grpc/internal/grpcsync
golang.org/x/text/transform
golang.org/x/text/unicode/bidi
golang.org/x/text/unicode/norm
golang.org/x/text/secure/bidirule
golang.org/x/net/http2/hpack
google.golang.org/grpc/internal/syscall
google.golang.org/grpc/keepalive
google.golang.org/grpc/peer
golang.org/x/net/idna
google.golang.org/grpc/stats
google.golang.org/grpc/tap
google.golang.org/grpc/naming
google.golang.org/grpc/resolver/dns
golang.org/x/net/http/httpguts
google.golang.org/grpc/resolver/passthrough
github.com/urfave/cli
golang.org/x/net/http2
gopkg.in/yaml.v2
google.golang.org/grpc/internal/transport
github.com/bndr/gotabulate
golang.org/x/net/context
google.golang.org/grpc
gitee.com/openeuler/A-Tune/api/profile
gitee.com/openeuler/A-Tune/common/utils
gitee.com/openeuler/A-Tune/common/config
gitee.com/openeuler/A-Tune/common/service
gitee.com/openeuler/A-Tune/common/client
gitee.com/openeuler/A-Tune/common/project
gitee.com/openeuler/A-Tune/modules/client/profile
command-line-arguments
go build -mod=vendor -v -ldflags '-s -w -extldflags "-static" -extldflags "-zrelro" -extldflags "-znow" -extldflags "-ftrapv" -X gitee.com/openeuler/A-Tune/common/config.Version=1.0.0(5735242)' -o pkg/atuned cmd/atuned/*.go
gitee.com/openeuler/A-Tune/common/sched
gitee.com/openeuler/A-Tune/common/registry
gitee.com/openeuler/A-Tune/common/http
gitee.com/openeuler/A-Tune/common/cpumask
gitee.com/openeuler/A-Tune/common/models
gitee.com/openeuler/A-Tune/common/topology
gitee.com/openeuler/A-Tune/common/system
gitee.com/openeuler/A-Tune/common/schedule/framework
gitee.com/openeuler/A-Tune/common/sysload
gitee.com/openeuler/A-Tune/common/schedule/filters
gitee.com/openeuler/A-Tune/common/schedule/plugins/powersave
xorm.io/builder
xorm.io/core
gitee.com/openeuler/A-Tune/common/schedule/plugins/numaaware
gitee.com/openeuler/A-Tune/common/schedule/plugins
github.com/mattn/go-sqlite3
github.com/go-xorm/xorm
gitee.com/openeuler/A-Tune/common/service/pyservice
gitee.com/openeuler/A-Tune/common/service/timer
github.com/coreos/go-systemd/daemon
github.com/golang/protobuf/protoc-gen-go/descriptor
google.golang.org/grpc/reflection/grpc_reflection_v1alpha
google.golang.org/grpc/reflection
gitee.com/openeuler/A-Tune/common/sqlstore
gitee.com/openeuler/A-Tune/common/schedule
gitee.com/openeuler/A-Tune/common/profile
gitee.com/openeuler/A-Tune/common/service/monitor
command-line-arguments
sqlite3 database/atuned.db ".read database/init.sql"
[root@openEuler A-Tune]#
```



```bash
[root@openEuler A-Tune]# make collector-install
BEGIN INSTALL A-Tune-Collector...
! pip3 show atune-collector || pip3 uninstall atune-collector -y
rm -rf collector
git clone https://gitee.com/openeuler/A-Tune-Collector.git collector
正克隆到 'collector'...
remote: Enumerating objects: 376, done.
remote: Total 376 (delta 0), reused 0 (delta 0), pack-reused 376
接收对象中: 100% (376/376), 400.08 KiB | 358.00 KiB/s, 完成.
处理 delta 中: 100% (223/223), 完成.
cd collector && python3 setup.py install
running install
running build
running build_py
creating build
creating build/lib
creating build/lib/atune_collector
copying atune_collector/__init__.py -> build/lib/atune_collector
copying atune_collector/collect_data.py -> build/lib/atune_collector
creating build/lib/atune_collector/plugin
copying atune_collector/plugin/plugin.py -> build/lib/atune_collector/plugin
copying atune_collector/plugin/__init__.py -> build/lib/atune_collector/plugin
creating build/lib/atune_collector/plugin/monitor
copying atune_collector/plugin/monitor/common.py -> build/lib/atune_collector/plugin/monitor
copying atune_collector/plugin/monitor/__init__.py -> build/lib/atune_collector/plugin/monitor
creating build/lib/atune_collector/plugin/configurator
copying atune_collector/plugin/configurator/exceptions.py -> build/lib/atune_collector/plugin/configurator
copying atune_collector/plugin/configurator/common.py -> build/lib/atune_collector/plugin/configurator
copying atune_collector/plugin/configurator/__init__.py -> build/lib/atune_collector/plugin/configurator
creating build/lib/atune_collector/plugin/monitor/processor
copying atune_collector/plugin/monitor/processor/stat.py -> build/lib/atune_collector/plugin/monitor/processor
copying atune_collector/plugin/monitor/processor/topo.py -> build/lib/atune_collector/plugin/monitor/processor
copying atune_collector/plugin/monitor/processor/__init__.py -> build/lib/atune_collector/plugin/monitor/processor
copying atune_collector/plugin/monitor/processor/info.py -> build/lib/atune_collector/plugin/monitor/processor
creating build/lib/atune_collector/plugin/monitor/storage
copying atune_collector/plugin/monitor/storage/iostat.py -> build/lib/atune_collector/plugin/monitor/storage
copying atune_collector/plugin/monitor/storage/topo.py -> build/lib/atune_collector/plugin/monitor/storage
copying atune_collector/plugin/monitor/storage/__init__.py -> build/lib/atune_collector/plugin/monitor/storage
creating build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/bandwidth.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/utilstat.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/topo.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/numainfo.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/vmstat.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/__init__.py -> build/lib/atune_collector/plugin/monitor/memory
copying atune_collector/plugin/monitor/memory/meminfo.py -> build/lib/atune_collector/plugin/monitor/memory
creating build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/filed.py -> build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/bios.py -> build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/tasks.py -> build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/ldavg.py -> build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/__init__.py -> build/lib/atune_collector/plugin/monitor/system
copying atune_collector/plugin/monitor/system/interrupts.py -> build/lib/atune_collector/plugin/monitor/system
creating build/lib/atune_collector/plugin/monitor/network
copying atune_collector/plugin/monitor/network/netstat.py -> build/lib/atune_collector/plugin/monitor/network
copying atune_collector/plugin/monitor/network/topo.py -> build/lib/atune_collector/plugin/monitor/network
copying atune_collector/plugin/monitor/network/__init__.py -> build/lib/atune_collector/plugin/monitor/network
copying atune_collector/plugin/monitor/network/info.py -> build/lib/atune_collector/plugin/monitor/network
copying atune_collector/plugin/monitor/network/netestat.py -> build/lib/atune_collector/plugin/monitor/network
creating build/lib/atune_collector/plugin/monitor/performance
copying atune_collector/plugin/monitor/performance/stat.py -> build/lib/atune_collector/plugin/monitor/performance
copying atune_collector/plugin/monitor/performance/top.py -> build/lib/atune_collector/plugin/monitor/performance
copying atune_collector/plugin/monitor/performance/__init__.py -> build/lib/atune_collector/plugin/monitor/performance
creating build/lib/atune_collector/plugin/monitor/process
copying atune_collector/plugin/monitor/process/sched.py -> build/lib/atune_collector/plugin/monitor/process
copying atune_collector/plugin/monitor/process/__init__.py -> build/lib/atune_collector/plugin/monitor/process
creating build/lib/atune_collector/plugin/configurator/ulimit
copying atune_collector/plugin/configurator/ulimit/__init__.py -> build/lib/atune_collector/plugin/configurator/ulimit
copying atune_collector/plugin/configurator/ulimit/ulimit.py -> build/lib/atune_collector/plugin/configurator/ulimit
creating build/lib/atune_collector/plugin/configurator/systemctl
copying atune_collector/plugin/configurator/systemctl/systemctl.py -> build/lib/atune_collector/plugin/configurator/systemctl
copying atune_collector/plugin/configurator/systemctl/__init__.py -> build/lib/atune_collector/plugin/configurator/systemctl
creating build/lib/atune_collector/plugin/configurator/file_config
copying atune_collector/plugin/configurator/file_config/fconfig.py -> build/lib/atune_collector/plugin/configurator/file_config
copying atune_collector/plugin/configurator/file_config/__init__.py -> build/lib/atune_collector/plugin/configurator/file_config
creating build/lib/atune_collector/plugin/configurator/script
copying atune_collector/plugin/configurator/script/script.py -> build/lib/atune_collector/plugin/configurator/script
copying atune_collector/plugin/configurator/script/__init__.py -> build/lib/atune_collector/plugin/configurator/script
creating build/lib/atune_collector/plugin/configurator/affinity
copying atune_collector/plugin/configurator/affinity/irq.py -> build/lib/atune_collector/plugin/configurator/affinity
copying atune_collector/plugin/configurator/affinity/processid.py -> build/lib/atune_collector/plugin/configurator/affinity
copying atune_collector/plugin/configurator/affinity/__init__.py -> build/lib/atune_collector/plugin/configurator/affinity
copying atune_collector/plugin/configurator/affinity/affinityutils.py -> build/lib/atune_collector/plugin/configurator/affinity
copying atune_collector/plugin/configurator/affinity/task.py -> build/lib/atune_collector/plugin/configurator/affinity
creating build/lib/atune_collector/plugin/configurator/kernel_config
copying atune_collector/plugin/configurator/kernel_config/kconfig.py -> build/lib/atune_collector/plugin/configurator/kernel_config
copying atune_collector/plugin/configurator/kernel_config/__init__.py -> build/lib/atune_collector/plugin/configurator/kernel_config
creating build/lib/atune_collector/plugin/configurator/sysctl
copying atune_collector/plugin/configurator/sysctl/__init__.py -> build/lib/atune_collector/plugin/configurator/sysctl
copying atune_collector/plugin/configurator/sysctl/sysctl.py -> build/lib/atune_collector/plugin/configurator/sysctl
creating build/lib/atune_collector/plugin/configurator/sysfs
copying atune_collector/plugin/configurator/sysfs/__init__.py -> build/lib/atune_collector/plugin/configurator/sysfs
copying atune_collector/plugin/configurator/sysfs/sysfs.py -> build/lib/atune_collector/plugin/configurator/sysfs
creating build/lib/atune_collector/plugin/configurator/bootloader
copying atune_collector/plugin/configurator/bootloader/bootutils.py -> build/lib/atune_collector/plugin/configurator/bootloader
copying atune_collector/plugin/configurator/bootloader/grub2.py -> build/lib/atune_collector/plugin/configurator/bootloader
copying atune_collector/plugin/configurator/bootloader/cmdline.py -> build/lib/atune_collector/plugin/configurator/bootloader
copying atune_collector/plugin/configurator/bootloader/__init__.py -> build/lib/atune_collector/plugin/configurator/bootloader
creating build/lib/atune_collector/plugin/configurator/bios
copying atune_collector/plugin/configurator/bios/bios.py -> build/lib/atune_collector/plugin/configurator/bios
copying atune_collector/plugin/configurator/bios/__init__.py -> build/lib/atune_collector/plugin/configurator/bios
running egg_info
creating atune_collector.egg-info
writing atune_collector.egg-info/PKG-INFO
writing dependency_links to atune_collector.egg-info/dependency_links.txt
writing requirements to atune_collector.egg-info/requires.txt
writing top-level names to atune_collector.egg-info/top_level.txt
writing manifest file 'atune_collector.egg-info/SOURCES.txt'
reading manifest file 'atune_collector.egg-info/SOURCES.txt'
reading manifest template 'MANIFEST.in'
writing manifest file 'atune_collector.egg-info/SOURCES.txt'
copying atune_collector/collect_data.json -> build/lib/atune_collector
creating build/lib/atune_collector/scripts
creating build/lib/atune_collector/scripts/blockdev
copying atune_collector/scripts/blockdev/get.sh -> build/lib/atune_collector/scripts/blockdev
copying atune_collector/scripts/blockdev/set.sh -> build/lib/atune_collector/scripts/blockdev
creating build/lib/atune_collector/scripts/ethtool
copying atune_collector/scripts/ethtool/common.sh -> build/lib/atune_collector/scripts/ethtool
copying atune_collector/scripts/ethtool/get.sh -> build/lib/atune_collector/scripts/ethtool
copying atune_collector/scripts/ethtool/set.sh -> build/lib/atune_collector/scripts/ethtool
creating build/lib/atune_collector/scripts/hdparm
copying atune_collector/scripts/hdparm/get.sh -> build/lib/atune_collector/scripts/hdparm
copying atune_collector/scripts/hdparm/set.sh -> build/lib/atune_collector/scripts/hdparm
creating build/lib/atune_collector/scripts/hinic
copying atune_collector/scripts/hinic/get.sh -> build/lib/atune_collector/scripts/hinic
copying atune_collector/scripts/hinic/set.sh -> build/lib/atune_collector/scripts/hinic
creating build/lib/atune_collector/scripts/hugepage
copying atune_collector/scripts/hugepage/get.sh -> build/lib/atune_collector/scripts/hugepage
copying atune_collector/scripts/hugepage/set.sh -> build/lib/atune_collector/scripts/hugepage
creating build/lib/atune_collector/scripts/ifconfig
copying atune_collector/scripts/ifconfig/get.sh -> build/lib/atune_collector/scripts/ifconfig
copying atune_collector/scripts/ifconfig/set.sh -> build/lib/atune_collector/scripts/ifconfig
creating build/lib/atune_collector/scripts/logind
copying atune_collector/scripts/logind/get.sh -> build/lib/atune_collector/scripts/logind
copying atune_collector/scripts/logind/set.sh -> build/lib/atune_collector/scripts/logind
creating build/lib/atune_collector/scripts/openssl_hpre
copying atune_collector/scripts/openssl_hpre/get.sh -> build/lib/atune_collector/scripts/openssl_hpre
copying atune_collector/scripts/openssl_hpre/hisi_openssl.cnf -> build/lib/atune_collector/scripts/openssl_hpre
copying atune_collector/scripts/openssl_hpre/openssl.cnf -> build/lib/atune_collector/scripts/openssl_hpre
copying atune_collector/scripts/openssl_hpre/set.sh -> build/lib/atune_collector/scripts/openssl_hpre
creating build/lib/atune_collector/scripts/prefetch
copying atune_collector/scripts/prefetch/get.sh -> build/lib/atune_collector/scripts/prefetch
copying atune_collector/scripts/prefetch/set.sh -> build/lib/atune_collector/scripts/prefetch
creating build/lib/atune_collector/scripts/rps_cpus
copying atune_collector/scripts/rps_cpus/get.sh -> build/lib/atune_collector/scripts/rps_cpus
copying atune_collector/scripts/rps_cpus/set.sh -> build/lib/atune_collector/scripts/rps_cpus
creating build/lib/atune_collector/scripts/sched_domain
copying atune_collector/scripts/sched_domain/get.sh -> build/lib/atune_collector/scripts/sched_domain
copying atune_collector/scripts/sched_domain/set.sh -> build/lib/atune_collector/scripts/sched_domain
creating build/lib/atune_collector/scripts/swap
copying atune_collector/scripts/swap/get.sh -> build/lib/atune_collector/scripts/swap
copying atune_collector/scripts/swap/set.sh -> build/lib/atune_collector/scripts/swap
copying atune_collector/plugin/configurator/bootloader/grub2.json -> build/lib/atune_collector/plugin/configurator/bootloader
running install_lib
creating /usr/local/lib/python3.7
creating /usr/local/lib/python3.7/site-packages
creating /usr/local/lib/python3.7/site-packages/atune_collector
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin
copying build/lib/atune_collector/plugin/plugin.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor
copying build/lib/atune_collector/plugin/monitor/processor/stat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor
copying build/lib/atune_collector/plugin/monitor/processor/topo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor
copying build/lib/atune_collector/plugin/monitor/processor/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor
copying build/lib/atune_collector/plugin/monitor/processor/info.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage
copying build/lib/atune_collector/plugin/monitor/storage/iostat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage
copying build/lib/atune_collector/plugin/monitor/storage/topo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage
copying build/lib/atune_collector/plugin/monitor/storage/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/bandwidth.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/utilstat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/topo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/numainfo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/vmstat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
copying build/lib/atune_collector/plugin/monitor/memory/meminfo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/filed.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/bios.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/tasks.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/ldavg.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/system/interrupts.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system
copying build/lib/atune_collector/plugin/monitor/common.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor
copying build/lib/atune_collector/plugin/monitor/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
copying build/lib/atune_collector/plugin/monitor/network/netstat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
copying build/lib/atune_collector/plugin/monitor/network/topo.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
copying build/lib/atune_collector/plugin/monitor/network/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
copying build/lib/atune_collector/plugin/monitor/network/info.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
copying build/lib/atune_collector/plugin/monitor/network/netestat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance
copying build/lib/atune_collector/plugin/monitor/performance/stat.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance
copying build/lib/atune_collector/plugin/monitor/performance/top.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance
copying build/lib/atune_collector/plugin/monitor/performance/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/process
copying build/lib/atune_collector/plugin/monitor/process/sched.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/process
copying build/lib/atune_collector/plugin/monitor/process/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/process
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/ulimit
copying build/lib/atune_collector/plugin/configurator/ulimit/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/ulimit
copying build/lib/atune_collector/plugin/configurator/ulimit/ulimit.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/ulimit
copying build/lib/atune_collector/plugin/configurator/exceptions.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/systemctl
copying build/lib/atune_collector/plugin/configurator/systemctl/systemctl.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/systemctl
copying build/lib/atune_collector/plugin/configurator/systemctl/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/systemctl
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/file_config
copying build/lib/atune_collector/plugin/configurator/file_config/fconfig.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/file_config
copying build/lib/atune_collector/plugin/configurator/file_config/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/file_config
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/script
copying build/lib/atune_collector/plugin/configurator/script/script.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/script
copying build/lib/atune_collector/plugin/configurator/script/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/script
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
copying build/lib/atune_collector/plugin/configurator/affinity/irq.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
copying build/lib/atune_collector/plugin/configurator/affinity/processid.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
copying build/lib/atune_collector/plugin/configurator/affinity/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
copying build/lib/atune_collector/plugin/configurator/affinity/affinityutils.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
copying build/lib/atune_collector/plugin/configurator/affinity/task.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/kernel_config
copying build/lib/atune_collector/plugin/configurator/kernel_config/kconfig.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/kernel_config
copying build/lib/atune_collector/plugin/configurator/kernel_config/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/kernel_config
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysctl
copying build/lib/atune_collector/plugin/configurator/sysctl/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysctl
copying build/lib/atune_collector/plugin/configurator/sysctl/sysctl.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysctl
copying build/lib/atune_collector/plugin/configurator/common.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator
copying build/lib/atune_collector/plugin/configurator/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysfs
copying build/lib/atune_collector/plugin/configurator/sysfs/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysfs
copying build/lib/atune_collector/plugin/configurator/sysfs/sysfs.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysfs
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
copying build/lib/atune_collector/plugin/configurator/bootloader/bootutils.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
copying build/lib/atune_collector/plugin/configurator/bootloader/grub2.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
copying build/lib/atune_collector/plugin/configurator/bootloader/cmdline.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
copying build/lib/atune_collector/plugin/configurator/bootloader/grub2.json -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
copying build/lib/atune_collector/plugin/configurator/bootloader/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader
creating /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bios
copying build/lib/atune_collector/plugin/configurator/bios/bios.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bios
copying build/lib/atune_collector/plugin/configurator/bios/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bios
copying build/lib/atune_collector/plugin/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector/plugin
copying build/lib/atune_collector/collect_data.json -> /usr/local/lib/python3.7/site-packages/atune_collector
copying build/lib/atune_collector/__init__.py -> /usr/local/lib/python3.7/site-packages/atune_collector
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hdparm
copying build/lib/atune_collector/scripts/hdparm/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hdparm
copying build/lib/atune_collector/scripts/hdparm/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hdparm
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/openssl_hpre
copying build/lib/atune_collector/scripts/openssl_hpre/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/openssl_hpre
copying build/lib/atune_collector/scripts/openssl_hpre/hisi_openssl.cnf -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/openssl_hpre
copying build/lib/atune_collector/scripts/openssl_hpre/openssl.cnf -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/openssl_hpre
copying build/lib/atune_collector/scripts/openssl_hpre/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/openssl_hpre
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/swap
copying build/lib/atune_collector/scripts/swap/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/swap
copying build/lib/atune_collector/scripts/swap/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/swap
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/prefetch
copying build/lib/atune_collector/scripts/prefetch/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/prefetch
copying build/lib/atune_collector/scripts/prefetch/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/prefetch
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hugepage
copying build/lib/atune_collector/scripts/hugepage/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hugepage
copying build/lib/atune_collector/scripts/hugepage/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hugepage
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/blockdev
copying build/lib/atune_collector/scripts/blockdev/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/blockdev
copying build/lib/atune_collector/scripts/blockdev/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/blockdev
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ethtool
copying build/lib/atune_collector/scripts/ethtool/common.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ethtool
copying build/lib/atune_collector/scripts/ethtool/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ethtool
copying build/lib/atune_collector/scripts/ethtool/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ethtool
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/rps_cpus
copying build/lib/atune_collector/scripts/rps_cpus/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/rps_cpus
copying build/lib/atune_collector/scripts/rps_cpus/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/rps_cpus
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/logind
copying build/lib/atune_collector/scripts/logind/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/logind
copying build/lib/atune_collector/scripts/logind/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/logind
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hinic
copying build/lib/atune_collector/scripts/hinic/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hinic
copying build/lib/atune_collector/scripts/hinic/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/hinic
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ifconfig
copying build/lib/atune_collector/scripts/ifconfig/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ifconfig
copying build/lib/atune_collector/scripts/ifconfig/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/ifconfig
creating /usr/local/lib/python3.7/site-packages/atune_collector/scripts/sched_domain
copying build/lib/atune_collector/scripts/sched_domain/set.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/sched_domain
copying build/lib/atune_collector/scripts/sched_domain/get.sh -> /usr/local/lib/python3.7/site-packages/atune_collector/scripts/sched_domain
copying build/lib/atune_collector/collect_data.py -> /usr/local/lib/python3.7/site-packages/atune_collector
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/plugin.py to plugin.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor/stat.py to stat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor/topo.py to topo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/processor/info.py to info.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage/iostat.py to iostat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage/topo.py to topo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/storage/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/bandwidth.py to bandwidth.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/utilstat.py to utilstat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/topo.py to topo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/numainfo.py to numainfo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/vmstat.py to vmstat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/memory/meminfo.py to meminfo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/filed.py to filed.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/bios.py to bios.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/tasks.py to tasks.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/ldavg.py to ldavg.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/system/interrupts.py to interrupts.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/common.py to common.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network/netstat.py to netstat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network/topo.py to topo.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network/info.py to info.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/network/netestat.py to netestat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance/stat.py to stat.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance/top.py to top.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/performance/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/process/sched.py to sched.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/monitor/process/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/ulimit/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/ulimit/ulimit.py to ulimit.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/exceptions.py to exceptions.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/systemctl/systemctl.py to systemctl.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/systemctl/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/file_config/fconfig.py to fconfig.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/file_config/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/script/script.py to script.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/script/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity/irq.py to irq.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity/processid.py to processid.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity/affinityutils.py to affinityutils.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/affinity/task.py to task.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/kernel_config/kconfig.py to kconfig.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/kernel_config/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysctl/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysctl/sysctl.py to sysctl.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/common.py to common.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysfs/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/sysfs/sysfs.py to sysfs.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader/bootutils.py to bootutils.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader/grub2.py to grub2.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader/cmdline.py to cmdline.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bootloader/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bios/bios.py to bios.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/configurator/bios/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/plugin/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/__init__.py to __init__.cpython-37.pyc
byte-compiling /usr/local/lib/python3.7/site-packages/atune_collector/collect_data.py to collect_data.cpython-37.pyc
running install_data
creating /etc/atune_collector
copying atune_collector/collect_data.json -> /etc/atune_collector
copying atune_collector/plugin/configurator/bootloader/grub2.json -> /etc/atune_collector
running install_egg_info
Copying atune_collector.egg-info to /usr/local/lib/python3.7/site-packages/atune_collector-0.1-py3.7.egg-info
running install_scripts
END INSTALL A-Tune-Collector...
[root@openEuler A-Tune]#
```



```bash
[root@openEuler A-Tune]# make install
BEGIN INSTALL A-Tune...
mkdir -p /usr/bin
mkdir -p /usr/lib/systemd/system
rm -rf /etc/atuned/
rm -rf /usr/lib/atuned/
rm -rf /usr/share/atuned/
rm -rf /usr/libexec/atuned/
rm -rf /var/lib/atuned/
rm -rf /var/run/atuned/
rm -rf /var/atuned/
mkdir -p /etc/atuned/tuning
mkdir -p /etc/atuned/rules
mkdir -p /etc/atuned/training
mkdir -p /etc/atuned/classification
mkdir -p /usr/lib/atuned/modules
mkdir -p /usr/lib/atuned/profiles
mkdir -p /usr/lib/atuned/training
mkdir -p /usr/share/atuned
mkdir -p /usr/libexec/atuned/analysis/models
mkdir -p /var/lib/atuned
mkdir -p /var/run/atuned
mkdir -p /var/atuned
mkdir -p /usr/share/bash-completion/completions
install -m 640 pkg/daemon_profile_server.so /usr/lib/atuned/modules
install -m 750 pkg/atune-adm /usr/bin
install -m 750 pkg/atuned /usr/bin
install -m 640 misc/atuned.service /usr/lib/systemd/system
install -m 640 misc/atuned.cnf /etc/atuned/
install -m 640 misc/engine.cnf /etc/atuned/
install -m 640 misc/ui.cnf /etc/atuned/
install -m 640 rules/tuning/tuning_rules.grl /etc/atuned/rules
install -m 640 misc/atune-engine.service /usr/lib/systemd/system
install -m 640 database/atuned.db /var/lib/atuned/
install -m 640 misc/atune-adm /usr/share/bash-completion/completions/
install -m 640 misc/atune-ui.service /usr/lib/systemd/system
\cp -rf analysis/* /usr/libexec/atuned/analysis/
chmod -R 750 /usr/libexec/atuned/analysis/
\cp -rf profiles/* /usr/lib/atuned/profiles/
chmod -R 750 /usr/lib/atuned/profiles/
END INSTALL A-Tune
cd /root/A-Tune/tools/ && python3 generate_models.py --model_path /usr/libexec/atuned/analysis/models 
the accuracy of random forest classifier is 1.0
The overall classifier has been generated.
the accuracy of random forest classifier is 1.0
The application classifiers have been generated.
the accuracy of svc classifier is 1.0
The application classifiers have been generated.
BEGIN GENERATE REST CERTS...
mkdir -p /etc/atuned/rest_certs
openssl genrsa -out /etc/atuned/rest_certs/ca.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
...................+++++
........................................................+++++
e is 65537 (0x010001)
openssl req -new -x509 -days 3650 -subj "/CN=ca" -key /etc/atuned/rest_certs/ca.key -out /etc/atuned/rest_certs/ca.crt
openssl genrsa -out /etc/atuned/rest_certs/server.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
...................................................+++++
......................................................................................+++++
e is 65537 (0x010001)
cp /etc/pki/tls/openssl.cnf /etc/atuned/rest_certs
openssl req -new -subj "/CN=localhost" -config /etc/atuned/rest_certs/openssl.cnf \
        -key /etc/atuned/rest_certs/server.key -out /etc/atuned/rest_certs/server.csr
openssl x509 -req -sha256 -CA /etc/atuned/rest_certs/ca.crt -CAkey /etc/atuned/rest_certs/ca.key -CAcreateserial -days 3650 \
        -extfile /etc/atuned/rest_certs/extfile.cnf -in /etc/atuned/rest_certs/server.csr -out /etc/atuned/rest_certs/server.crt
Signature ok
subject=CN = localhost
Getting CA Private Key
rm -rf /etc/atuned/rest_certs/*.srl /etc/atuned/rest_certs/*.csr /etc/atuned/rest_certs/*.cnf
END GENERATE REST CERTS
BEGIN GENERATE ENGINE CERTS...
mkdir -p /etc/atuned/engine_certs
Generating RSA private key, 2048 bit long modulus (2 primes)
................................................................+++++
..................+++++
e is 65537 (0x010001)
cp /etc/pki/tls/openssl.cnf /etc/atuned/engine_certs
Generating RSA private key, 2048 bit long modulus (2 primes)
...+++++
..........................................+++++
e is 65537 (0x010001)
Signature ok
subject=CN = localhost
Getting CA Private Key
Generating RSA private key, 2048 bit long modulus (2 primes)
.................................................................................................................................................................+++++
..........................................+++++
e is 65537 (0x010001)
Signature ok
subject=CN = localhost
Getting CA Private Key
rm -rf /etc/atuned/engine_certs/*.srl /etc/atuned/engine_certs/*.csr /etc/atuned/engine_certs/*.cnf
END GENERATE ENGINE CERTS
\cp -rf tuning/yamls/* /etc/atuned/tuning
chmod -R 750 /etc/atuned/tuning
cd /root/A-Tune/tools/ && python3 generate_tuning_file.py -d /etc/atuned/tuning
START UPDATE ATUNED CONFIG...
NIC *ens33* is used as default network interface for data collection.
DEV *sda* is used as default disk drive for data collection.
modify /etc/atuned/atuned.cnf if wrong devices are used.
END UPDATE ATUNED CONFIG
[root@openEuler A-Tune]#
```



# 三、部署A-Tune服务

## 1、配置介绍

​		A-Tune配置文件位于`/etc/atuned/atuned.cnf`，配置项说明：

### （1）A-Tune服务启动配置

​	🔒 `protocol`：系统grpc服务使用的协议，unix或tcp，unix为本地socket通信方式，tcp为socket监听端口方式。默认为unix。

​	🔒 `address`：系统grpc服务的侦听地址，默认为unix socket，若为分布式部署，需修改为侦听的ip地址。

​	🔒 `port`：系统grpc服务的侦听端口，范围为0~65535未使用的端口。如果protocol配置是unix，则不需要配置。

​	🔒 `rest_port`：系统restservice的侦听端口, 范围为0~65535未使用的端口。

​	🔒 `sample_num`：系统执行analysis流程时采集样本的数量。

### （2）system信息

​		system为系统执行相关的优化需要用到的参数信息，必须根据系统实际情况进行修改。

​	🔒 `disk`：执行analysis流程时需要采集的对应磁盘的信息或执行磁盘相关优化时需要指定的磁盘

​	🔒` network`：执行analysis时需要采集的对应的网卡的信息或执行网卡相关优化时需要指定的网卡

​	🔒 `user`：执行ulimit相关优化时用到的用户名。目前只支持root用户

​	🔒 `tls`：开启A-Tune的gRPC和http服务SSL/TLS证书校验，默认不开启。开启TLS后atune-adm命令在使用前需要设置以下环境变量方可与服务端进行通讯：

```ini
export ATUNE_TLS=yes
export ATUNE_CLICERT=<客户端证书路径>
```

​	🔒 `tlsservercertfile`：gPRC服务端证书路径。

​	🔒 `tlsserverkeyfile`：gPRC服务端秘钥路径。

​	🔒 `tlshttpcertfile`：http服务端证书路径。

​	🔒 `tlshttpkeyfile`：http服务端秘钥路径。

​	🔒 `tlshttpcacertfile`：http服务端CA证书路径。

### （3）日志信息

​		根据情况修改日志的路径和级别，默认的日志信息在`/var/log/messages`中。

### （4）monitor信息

​		为系统启动时默认采集的系统硬件信息。



## 2、查看配置信息

```bash
[root@openEuler A-Tune]# cat /etc/atuned/atuned.cnf
# Copyright (c) 2019 Huawei Technologies Co., Ltd.
# A-Tune is licensed under the Mulan PSL v2.
# You can use this software according to the terms and conditions of the Mulan PSL v2.
# You may obtain a copy of Mulan PSL v2 at:
#     http://license.coscl.org.cn/MulanPSL2
# THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT, MERCHANTABILITY OR FIT FOR A PARTICULAR
# PURPOSE.
# See the Mulan PSL v2 for more details.
# Create: 2019-10-29

#################################### server ###############################
# atuned config
[server]
# the protocol grpc server running on
# ranges: unix or tcp
protocol = unix

# the address that the grpc server to bind to
# default is unix socket /var/run/atuned/atuned.sock
# ranges: /var/run/atuned/atuned.sock or ip address
address = /var/run/atuned/atuned.sock

# the atune nodes in cluster mode, separated by commas
# it is valid when protocol is tcp
# connect = ip01,ip02,ip03

# the atuned grpc listening port
# the port can be set between 0 to 65535 which not be used
# port = 60001

# the rest service listening port, default is 8383
# the port can be set between 0 to 65535 which not be used
rest_host = localhost
rest_port = 8383

# the tuning optimizer host and port, start by engine.service
# if engine_host is same as rest_host, two ports cannot be same
# the port can be set between 0 to 65535 which not be used
engine_host = localhost
engine_port = 3838

# when run analysis command, the numbers of collected data.
# default is 20
sample_num = 20

# interval for collecting data, default is 5s
interval = 5

# enable gRPC authentication SSL/TLS
# default is false
# grpc_tls = false
# tlsservercafile = /etc/atuned/grpc_certs/ca.crt
# tlsservercertfile = /etc/atuned/grpc_certs/server.crt
# tlsserverkeyfile = /etc/atuned/grpc_certs/server.key

# enable rest server authentication SSL/TLS
# default is true
rest_tls = true
tlsrestcacertfile = /etc/atuned/rest_certs/ca.crt
tlsrestservercertfile = /etc/atuned/rest_certs/server.crt
tlsrestserverkeyfile = /etc/atuned/rest_certs/server.key

# enable engine server authentication SSL/TLS
# default is true
engine_tls = true
tlsenginecacertfile = /etc/atuned/engine_certs/ca.crt
tlsengineclientcertfile = /etc/atuned/engine_certs/client.crt
tlsengineclientkeyfile = /etc/atuned/engine_certs/client.key

#################################### log ###############################
[log]
# either "debug", "info", "warn", "error", "critical", default is "info"
level = info

#################################### monitor ###############################
[monitor]
# with the module and format of the MPI, the format is {module}_{purpose}
# the module is Either "mem", "net", "cpu", "storage"
# the purpose is "topo"
module = mem_topo, cpu_topo

#################################### system ###############################
# you can add arbitrary key-value here, just like key = value
# you can use the key in the profile
[system]
# the disk to be analysis
disk = sda

# the network to be analysis
network = ens33

user = root

#################################### tuning ###############################
# tuning configs
[tuning]
noise = 0.000000001
sel_feature = false
[root@openEuler A-Tune]#
```



## 3、启动和管理A-Tune服务

### （1）启动A-Tune服务

​		加载服务

```bash
[root@openEuler A-Tune]# systemctl daemon-reload
```

​		启动服务

```bash
[root@openEuler A-Tune]# systemctl start atuned
[root@openEuler A-Tune]# systemctl start atune-engine
```

​		查看服务状态

```bash
[root@openEuler A-Tune]# systemctl status atuned
● atuned.service - A-Tune Daemon
   Loaded: loaded (/usr/lib/systemd/system/atuned.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2023-01-13 11:55:24 CST; 23s ago
 Main PID: 17188 (atuned)
    Tasks: 11
   Memory: 75.0M
   CGroup: /system.slice/atuned.service
           ├─17188 /usr/bin/atuned
           └─17194 /usr/bin/python3 /usr/libexec/atuned/analysis/app_rest.py /etc/atuned/atuned.cnf

1月 13 11:55:20 openEuler atuned[17188]: time="2023-01-13T11:55:20+08:00" level=info msg="Connecting to DB: /var/lib/atuned/>
1月 13 11:55:20 openEuler atuned[17188]: time="2023-01-13T11:55:20+08:00" level=info msg="initializing service: monitor" fil>
1月 13 11:55:20 openEuler atuned[17188]: time="2023-01-13T11:55:20+08:00" level=info msg="initializing service: pyengine" fi>
1月 13 11:55:20 openEuler atuned[17188]: time="2023-01-13T11:55:20+08:00" level=info msg="load profile service successfully\>
1月 13 11:55:20 openEuler atuned[17188]: time="2023-01-13T11:55:20+08:00" level=info msg="Create opt.profile service success>
1月 13 11:55:24 openEuler atuned[17194]: 2023-01-13 11:55:24,906 [INFO] monitor [/usr/libexec/atuned/analysis/../analysis/at>
1月 13 11:55:24 openEuler atuned[17188]: time="2023-01-13T11:55:24+08:00" level=info msg="pyservice has been started" file=">
1月 13 11:55:24 openEuler systemd[1]: Started A-Tune Daemon.
1月 13 11:55:26 openEuler atuned[17194]: 2023-01-13 11:55:26,240 [INFO] monitor [/usr/libexec/atuned/analysis/../analysis/at>
1月 13 11:55:26 openEuler atuned[17188]: time="2023-01-13T11:55:26+08:00" level=warning msg="no active profile or more than >
[root@openEuler A-Tune]#
```



### （2）管理A-Tune

​		使用A-Tune需要使用root权限。

​		atune-adm支持的命令可以通过`atune-adm help/–help/-h`查询。

​		使用方法中所有命令的使用举例都是在单机部署模式下，如果是在分布式部署模式下，需要指定服务器IP和端口号。

​		`define`、`update`、`undefine`、`collection`、`train`、`upgrade`不支持远程执行。

​		命令格式中，`[ ] `表示参数可选，`<> `表示参数必选，具体参数由实际情况确定。

​		命令格式中，各命令含义如下：

```bash
WORKLOAD_TYPE：用户自定义负载类型的名称，负载支持的类型参考list命令查询结果。
PROFILE_NAME：用户自定义profile的名称
PROFILE_PATH：用户自定义profile的路径
```

#### 查询负载类型：

​		查询系统当前支持的workload_type和对应的profile，以及当前处于active状态的profile：

```bash
[root@openEuler A-Tune]# atune-adm list

Support profiles:
+---------------------------------------------+-----------+
| ProfileName                                 | Active    |
+=============================================+===========+
| arm-native-android-container-robox          | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-fio               | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-lmbench           | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-netperf           | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-stream            | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-unixbench         | false     |
+---------------------------------------------+-----------+
| basic-test-suite-speccpu-speccpu2006        | false     |
+---------------------------------------------+-----------+
| basic-test-suite-specjbb-specjbb2015        | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-hdfs-dfsio-hdd              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-hdfs-dfsio-ssd              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-bayesian              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-kmeans                | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql1                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql10                 | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql2                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql3                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql4                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql5                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql6                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql7                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql8                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql9                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-tersort               | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-wordcount             | false     |
+---------------------------------------------+-----------+
| cloud-compute-kvm-host                      | false     |
+---------------------------------------------+-----------+
| database-mariadb-2p-tpcc-c3                 | false     |
+---------------------------------------------+-----------+
| database-mariadb-4p-tpcc-c3                 | false     |
+---------------------------------------------+-----------+
| database-mongodb-2p-sysbench                | false     |
+---------------------------------------------+-----------+
| database-mysql-2p-sysbench-hdd              | false     |
+---------------------------------------------+-----------+
| database-mysql-2p-sysbench-ssd              | false     |
+---------------------------------------------+-----------+
| database-postgresql-2p-sysbench-hdd         | false     |
+---------------------------------------------+-----------+
| database-postgresql-2p-sysbench-ssd         | false     |
+---------------------------------------------+-----------+
| default-default                             | false     |
+---------------------------------------------+-----------+
| docker-mariadb-2p-tpcc-c3                   | false     |
+---------------------------------------------+-----------+
| docker-mariadb-4p-tpcc-c3                   | false     |
+---------------------------------------------+-----------+
| encryption-AES-chauffeur                    | false     |
+---------------------------------------------+-----------+
| encryption-MD5-chauffeur                    | false     |
+---------------------------------------------+-----------+
| encryption-RSAPublic-chauffeur              | false     |
+---------------------------------------------+-----------+
| hpc-gatk4-human-genome                      | false     |
+---------------------------------------------+-----------+
| in-memory-database-redis-redis-benchmark    | false     |
+---------------------------------------------+-----------+
| middleware-dubbo-dubbo-benchmark            | false     |
+---------------------------------------------+-----------+
| storage-ceph-vdbench-hdd                    | false     |
+---------------------------------------------+-----------+
| storage-ceph-vdbench-ssd                    | false     |
+---------------------------------------------+-----------+
| virtualization-consumer-cloud-olc           | false     |
+---------------------------------------------+-----------+
| virtualization-mariadb-2p-tpcc-c3           | false     |
+---------------------------------------------+-----------+
| virtualization-mariadb-4p-tpcc-c3           | false     |
+---------------------------------------------+-----------+
| web-apache-traffic-server-spirent-pingpo    | false     |
+---------------------------------------------+-----------+
| web-nginx-http-long-connection              | false     |
+---------------------------------------------+-----------+
| web-nginx-http-short-connection             | false     |
+---------------------------------------------+-----------+
| web-nginx-https-long-connection             | false     |
+---------------------------------------------+-----------+
| web-nginx-https-short-connection            | false     |
+---------------------------------------------+-----------+
| web-tomcat-http-connection                  | false     |
+---------------------------------------------+-----------+

[root@openEuler A-Tune]#
```



#### **查询profile信息**

​		查看workload_type对应的profile内容：

```bash
atune-adm info <WORKLOAD_TYPE_>_
```

例如查看`basic-test-suite-baseline-lmbench`的profile内容：

```bash
[root@openEuler A-Tune]# atune-adm info basic-test-suite-baseline-lmbench

*** basic-test-suite-baseline-lmbench:
# Copyright (c) 2020 Huawei Technologies Co., Ltd.
# A-Tune is licensed under the Mulan PSL v2.
# You can use this software according to the terms and conditions of the Mulan PSL v2.
# You may obtain a copy of Mulan PSL v2 at:
#     http://license.coscl.org.cn/MulanPSL2
# THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT, MERCHANTABILITY OR FIT FOR A PARTICULAR
# PURPOSE.
# See the Mulan PSL v2 for more details.
# Create: 2020-06-18
#
# baseline test lmbench A-Tune configuration
#
[main]
include = include-basic-test-suite

[kernel_config]
#TODO CONFIG

[bios]
#TODO CONFIG

[bootloader.grub2]
#TODO CONFIG

[sysfs]
#TODO CONFIG

[systemctl]
#TODO CONFIG

[sysctl]
#TODO CONFIG

[script]
#TODO CONFIG

[ulimit]
#TODO CONFIG

[schedule_policy]
#TODO CONFIG

[check]
#TODO CONFIG

[tip]
mkfs.ext4 -b 16384 -O ^metadata_csum /dev/{disk} and restart the host = block
modify compilation options from -O to -O2 = compile
[root@openEuler A-Tune]#
```

#### 激活profile

​		手动激活workload_type对应的profile，使得workload_type处于active状态：

```bash
atune-adm profile [ProfileName]
```

例如想要激活`**basic-test-suite-baseline-lmbench`：

```bash
[root@openEuler A-Tune]# atune-adm profile basic-test-suite-baseline-lmbench
 [ SUGGEST] Bios                                     please change the BIOS configuration Stream Write Mode to Allocate share LLC,please change the BIOS configuration Custom Refresh Rate to 64ms,please change the BIOS configuration Power Policy to Performance,please change the BIOS configuration NUMA to Enable 
 [ SUGGEST] Bootloader                               Need reboot to make the config change of grub2 effect. 
 [ SUCCESS] memory                                   memory num is 1, the memory slot is full           
 [ SUGGEST] Kernel                                   please change the kernel configuration CONFIG_CGROUP_FILES to n,please change the kernel configuration CONFIG_SLUB_DEBUG to n,please change the kernel configuration CONFIG_PM_DEBUG to n,please change the kernel configuration CONFIG_PM_SLEEP_DEBUG to n,please change the kernel configuration CONFIG_STACKPROTECTOR to n,please change the kernel configuration CONFIG_STACKPROTECTOR_STRONG to n,please change the kernel configuration CONFIG_VMAP_STACK to n,please change the kernel configuration CONFIG_BLK_DEBUG_FS to n,please change the kernel configuration CONFIG_BLK_DEBUG_FS_ZONED to n,please change the kernel configuration CONFIG_NET_DROP_MONITOR to n,please change the kernel configuration CONFIG_DM_DEBUG to n,please change the kernel configuration CONFIG_MLX4_DEBUG to n,please change the kernel configuration CONFIG_VIDEO_ADV_DEBUG to n,please change the kernel configuration CONFIG_INFINIBAND_IPOIB_DEBUG to n,please change the kernel configuration CONFIG_NFS_DEBUG to n,please change the kernel configuration CONFIG_SUNRPC_DEBUG to n,please change the kernel configuration CONFIG_CIFS_DEBUG to n,please change the kernel configuration CONFIG_BINARY_PRINTF to n,please change the kernel configuration CONFIG_DEBUG_INFO_DWARF4 to n,please change the kernel configuration CONFIG_DEBUG_MEMORY_INIT to n,please change the kernel configuration CONFIG_SCHED_DEBUG to n,please change the kernel configuration CONFIG_DEBUG_BUGVERBOSE to n,please change the kernel configuration CONFIG_DEBUG_LIST to n,please change the kernel configuration CONFIG_TRACEPOINTS to n,please change the kernel configuration CONFIG_NOP_TRACER to n,please change the kernel configuration CONFIG_TRACER_MAX_TRACE to n,please change the kernel configuration CONFIG_TRACE_CLOCK to n,please change the kernel configuration CONFIG_RING_BUFFER to n,please change the kernel configuration CONFIG_EVENT_TRACING to n,please change the kernel configuration CONFIG_CONTEXT_SWITCH_TRACER to n,please change the kernel configuration CONFIG_TRACING to n,please change the kernel configuration CONFIG_GENERIC_TRACER to n,please change the kernel configuration CONFIG_FTRACE to n,please change the kernel configuration CONFIG_FUNCTION_TRACER to n,please change the kernel configuration CONFIG_FUNCTION_GRAPH_TRACER to n,please change the kernel configuration CONFIG_SCHED_TRACER to n,please change the kernel configuration CONFIG_HWLAT_TRACER to n,please change the kernel configuration CONFIG_FTRACE_SYSCALLS to n,please change the kernel configuration CONFIG_TRACER_SNAPSHOT to n,please change the kernel configuration CONFIG_BRANCH_PROFILE_NONE to n,please change the kernel configuration CONFIG_STACK_TRACER to n,please change the kernel configuration CONFIG_BLK_DEV_IO_TRACE to n,please change the kernel configuration CONFIG_KPROBE_EVENTS to n,please change the kernel configuration CONFIG_UPROBE_EVENTS to n,please change the kernel configuration CONFIG_BPF_EVENTS to n,please change the kernel configuration CONFIG_PROBE_EVENTS to n,please change the kernel configuration CONFIG_DYNAMIC_FTRACE to n,please change the kernel configuration CONFIG_FTRACE_MCOUNT_RECORD to n,please change the kernel configuration CONFIG_TRACING_MAP to n,please change the kernel configuration CONFIG_HIST_TRIGGERS to n,please change the kernel configuration CONFIG_RING_BUFFER_BENCHMARK to n,please change the kernel configuration CONFIG_DEBUG_ALIGN_RODATA to y,please change the kernel configuration CONFIG_NUMA_AWARE_SPINLOCKS to y 
 [ SUGGEST] Profile                                  please exec source /etc/profile to make the configuration take effect 
 [ FAILED ] Script                                   Command '['/usr/local/lib/python3.7/site-packages/atune_collector/scripts/prefetch/set.sh', 'on']' returned non-zero exit status 3. 
 [ SUCCESS] Sysctl                                   vm.dirty_background_ratio,vm.swappiness,net.core.busy_read,vm.min_free_kbytes,kernel.sched_autogroup_enabled 
 [ SUCCESS] Sysfs                                    kernel/mm/transparent_hugepage/defrag,kernel/mm/transparent_hugepage/enabled,block/sda/queue/read_ahead_kb 
 [ SUCCESS] Systemctl                                firewalld,sysmonitor,irqbalance,tuned              
 [ SUGGEST] block                                    mkfs.ext4 -b 16384 -O ^metadata_csum /dev/sda and restart the host 
 [ SUGGEST] compile                                  modify compilation options from -O to -O2          
 [ SUGGEST] kernel                                   SELinux provides extra control and security features to linux kernel. Disabling SELinux will improve the performance but may cause security risks. 
 [ SUGGEST] kernel                                   Audit is a system to collect security information. Disabling audit will improve the performance but may cause security risks. 
[root@openEuler A-Tune]#
```

​		会显示如上一系列提示信息以及安全警告信息。

​		验证是否激活成功：

```bash
[root@openEuler A-Tune]# atune-adm list

Support profiles:
+---------------------------------------------+-----------+
| ProfileName                                 | Active    |
+=============================================+===========+
| arm-native-android-container-robox          | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-fio               | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-lmbench           | true      |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-netperf           | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-stream            | false     |
+---------------------------------------------+-----------+
| basic-test-suite-baseline-unixbench         | false     |
+---------------------------------------------+-----------+
| basic-test-suite-speccpu-speccpu2006        | false     |
+---------------------------------------------+-----------+
| basic-test-suite-specjbb-specjbb2015        | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-hdfs-dfsio-hdd              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-hdfs-dfsio-ssd              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-bayesian              | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-kmeans                | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql1                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql10                 | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql2                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql3                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql4                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql5                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql6                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql7                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql8                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-sql9                  | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-tersort               | false     |
+---------------------------------------------+-----------+
| big-data-hadoop-spark-wordcount             | false     |
+---------------------------------------------+-----------+
| cloud-compute-kvm-host                      | false     |
+---------------------------------------------+-----------+
| database-mariadb-2p-tpcc-c3                 | false     |
+---------------------------------------------+-----------+
| database-mariadb-4p-tpcc-c3                 | false     |
+---------------------------------------------+-----------+
| database-mongodb-2p-sysbench                | false     |
+---------------------------------------------+-----------+
| database-mysql-2p-sysbench-hdd              | false     |
+---------------------------------------------+-----------+
| database-mysql-2p-sysbench-ssd              | false     |
+---------------------------------------------+-----------+
| database-postgresql-2p-sysbench-hdd         | false     |
+---------------------------------------------+-----------+
| database-postgresql-2p-sysbench-ssd         | false     |
+---------------------------------------------+-----------+
| default-default                             | false     |
+---------------------------------------------+-----------+
| docker-mariadb-2p-tpcc-c3                   | false     |
+---------------------------------------------+-----------+
| docker-mariadb-4p-tpcc-c3                   | false     |
+---------------------------------------------+-----------+
| encryption-AES-chauffeur                    | false     |
+---------------------------------------------+-----------+
| encryption-MD5-chauffeur                    | false     |
+---------------------------------------------+-----------+
| encryption-RSAPublic-chauffeur              | false     |
+---------------------------------------------+-----------+
| hpc-gatk4-human-genome                      | false     |
+---------------------------------------------+-----------+
| in-memory-database-redis-redis-benchmark    | false     |
+---------------------------------------------+-----------+
| middleware-dubbo-dubbo-benchmark            | false     |
+---------------------------------------------+-----------+
| storage-ceph-vdbench-hdd                    | false     |
+---------------------------------------------+-----------+
| storage-ceph-vdbench-ssd                    | false     |
+---------------------------------------------+-----------+
| virtualization-consumer-cloud-olc           | false     |
+---------------------------------------------+-----------+
| virtualization-mariadb-2p-tpcc-c3           | false     |
+---------------------------------------------+-----------+
| virtualization-mariadb-4p-tpcc-c3           | false     |
+---------------------------------------------+-----------+
| web-apache-traffic-server-spirent-pingpo    | false     |
+---------------------------------------------+-----------+
| web-nginx-http-long-connection              | false     |
+---------------------------------------------+-----------+
| web-nginx-http-short-connection             | false     |
+---------------------------------------------+-----------+
| web-nginx-https-long-connection             | false     |
+---------------------------------------------+-----------+
| web-nginx-https-short-connection            | false     |
+---------------------------------------------+-----------+
| web-tomcat-http-connection                  | false     |
+---------------------------------------------+-----------+

[root@openEuler A-Tune]#
```

​		`basic-test-suite-baseline-lmbench`一行活动状态为`true`，表明该 profile 已被成功激活。

​		此外，还有对profile的更新、回滚，以及系统信息查询，用法都比较简单在此不再一一举例。

# 五、调优实际操作（以优化nginx为例）

​		首先进入示例目录：

```bash
[root@openEuler A-Tune]# ls
analysis           cmd        Dockerfile-atune-engine  go.sum    modules   README.md     tests    vendor
anomaly_detection  collector  Documentation            License   optcfg    README-zh.md  TODO.md
api                common     examples                 Makefile  pkg       rules         tools
AUTHORS            database   go.mod                   misc      profiles  scripts       tuning
[root@openEuler A-Tune]# cd examples
[root@openEuler examples]# ls
Anomaly_Detection  tuning
[root@openEuler examples]# cd tuning
[root@openEuler tuning]# ls
compress                 go_gc   key_parameters_select          memory          redis             tomcat
compress_Except_example  Hive    key_parameters_select_variant  mysql_sysbench  spark
fio                      iozone  mariadb                        nginx           tensorflow_train
gcc_compile              kafka   memcached                      openGauss       tidb
[root@openEuler tuning]#
```

​		实例目录位于`/root/A-Tune/examples/tuning`，该目录下有若干实例，我们进入`nginx` 目录，并准备环境：

```bash
[root@openEuler tuning]# cd nginx
[root@openEuler nginx]# pwd
/root/A-Tune/examples/tuning/nginx
[root@openEuler nginx]# ls
nginx_benchmark.sh  nginx_client.yaml  nginx_http_long_client.yaml  prepare.sh  README
[root@openEuler nginx]#
```

​		**调优环境**：

​		执行sh命令后，系统会自动安装nginx、启动nginx、更新nginx客户端：

```bash
[root@openEuler nginx]# sh prepare.sh 
install nginx
Package gnutls-3.6.9-5.oe1.x86_64 is already installed.
Package libev-4.24-11.oe1.x86_64 is already installed.
Dependencies resolved.
===========================================================================================================================
 Package                                  Architecture        Version                        Repository               Size
===========================================================================================================================
Installing:
 gnutls-devel                             x86_64              3.6.9-5.oe1                    OS                       69 k
 libev-devel                              x86_64              4.24-11.oe1                    everything               19 k
 nginx                                    x86_64              1:1.16.1-2.oe1                 everything              480 k
Installing dependencies:
 gd                                       x86_64              2.2.5-6.oe1                    OS                      142 k
 gmp-c++                                  x86_64              1:6.1.2-10.oe1                 OS                       16 k
 gmp-devel                                x86_64              1:6.1.2-10.oe1                 OS                      441 k
 gperftools-libs                          x86_64              2.7-7.oe1                      OS                      267 k
 libtasn1-devel                           x86_64              4.13-7.oe1                     OS                       12 k
 libunwind                                x86_64              1.3.1-3.oe1                    OS                       54 k
 libwebp                                  x86_64              1.0.0-5.oe1                    OS                      246 k
 libxslt                                  x86_64              1.1.32-7.oe1                   OS                      233 k
 nettle-devel                             x86_64              3.4.1rc1-4.oe1                 OS                      267 k
 p11-kit-devel                            x86_64              0.23.14-6.oe1                  OS                       78 k
 nginx-all-modules                        noarch              1:1.16.1-2.oe1                 everything              7.7 k
 nginx-filesystem                         noarch              1:1.16.1-2.oe1                 everything              8.8 k
 nginx-mod-http-image-filter              x86_64              1:1.16.1-2.oe1                 everything               17 k
 nginx-mod-http-perl                      x86_64              1:1.16.1-2.oe1                 everything               26 k
 nginx-mod-http-xslt-filter               x86_64              1:1.16.1-2.oe1                 everything               16 k
 nginx-mod-mail                           x86_64              1:1.16.1-2.oe1                 everything               45 k
 nginx-mod-stream                         x86_64              1:1.16.1-2.oe1                 everything               68 k

Transaction Summary
===========================================================================================================================
Install  20 Packages

Total download size: 2.5 M
Installed size: 9.0 M
Downloading Packages:
(1/20): gmp-c++-6.1.2-10.oe1.x86_64.rpm                                                    323 kB/s |  16 kB     00:00    
(2/20): gnutls-devel-3.6.9-5.oe1.x86_64.rpm                                                2.2 MB/s |  69 kB     00:00    
(3/20): gd-2.2.5-6.oe1.x86_64.rpm                                                          1.4 MB/s | 142 kB     00:00    
(4/20): gmp-devel-6.1.2-10.oe1.x86_64.rpm                                                  4.2 MB/s | 441 kB     00:00    
(5/20): gperftools-libs-2.7-7.oe1.x86_64.rpm                                               8.1 MB/s | 267 kB     00:00    
(6/20): libtasn1-devel-4.13-7.oe1.x86_64.rpm                                               719 kB/s |  12 kB     00:00    
(7/20): libunwind-1.3.1-3.oe1.x86_64.rpm                                                   3.6 MB/s |  54 kB     00:00    
(8/20): libxslt-1.1.32-7.oe1.x86_64.rpm                                                     13 MB/s | 233 kB     00:00    
(9/20): nettle-devel-3.4.1rc1-4.oe1.x86_64.rpm                                              15 MB/s | 267 kB     00:00    
(10/20): p11-kit-devel-0.23.14-6.oe1.x86_64.rpm                                            4.8 MB/s |  78 kB     00:00    
(11/20): libev-devel-4.24-11.oe1.x86_64.rpm                                                1.3 MB/s |  19 kB     00:00    
(12/20): nginx-all-modules-1.16.1-2.oe1.noarch.rpm                                         489 kB/s | 7.7 kB     00:00    
(13/20): nginx-1.16.1-2.oe1.x86_64.rpm                                                      15 MB/s | 480 kB     00:00    
(14/20): nginx-filesystem-1.16.1-2.oe1.noarch.rpm                                          616 kB/s | 8.8 kB     00:00    
(15/20): libwebp-1.0.0-5.oe1.x86_64.rpm                                                    3.2 MB/s | 246 kB     00:00    
(16/20): nginx-mod-http-image-filter-1.16.1-2.oe1.x86_64.rpm                               1.1 MB/s |  17 kB     00:00    
(17/20): nginx-mod-http-perl-1.16.1-2.oe1.x86_64.rpm                                       1.7 MB/s |  26 kB     00:00    
(18/20): nginx-mod-http-xslt-filter-1.16.1-2.oe1.x86_64.rpm                                998 kB/s |  16 kB     00:00    
(19/20): nginx-mod-mail-1.16.1-2.oe1.x86_64.rpm                                            2.9 MB/s |  45 kB     00:00    
(20/20): nginx-mod-stream-1.16.1-2.oe1.x86_64.rpm                                          3.9 MB/s |  68 kB     00:00    
---------------------------------------------------------------------------------------------------------------------------
Total                                                                                       12 MB/s | 2.5 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                   1/1 
  Running scriptlet: nginx-filesystem-1:1.16.1-2.oe1.noarch                                                           1/20 
  Installing       : nginx-filesystem-1:1.16.1-2.oe1.noarch                                                           1/20 
  Installing       : p11-kit-devel-0.23.14-6.oe1.x86_64                                                               2/20 
  Installing       : libxslt-1.1.32-7.oe1.x86_64                                                                      3/20 
  Running scriptlet: libxslt-1.1.32-7.oe1.x86_64                                                                      3/20 
  Installing       : libwebp-1.0.0-5.oe1.x86_64                                                                       4/20 
  Installing       : gd-2.2.5-6.oe1.x86_64                                                                            5/20 
  Running scriptlet: gd-2.2.5-6.oe1.x86_64                                                                            5/20 
  Running scriptlet: libunwind-1.3.1-3.oe1.x86_64                                                                     6/20 
  Installing       : libunwind-1.3.1-3.oe1.x86_64                                                                     6/20 
  Installing       : gperftools-libs-2.7-7.oe1.x86_64                                                                 7/20 
  Installing       : nginx-mod-http-perl-1:1.16.1-2.oe1.x86_64                                                        8/20 
  Running scriptlet: nginx-mod-http-perl-1:1.16.1-2.oe1.x86_64                                                        8/20 
  Installing       : nginx-mod-http-xslt-filter-1:1.16.1-2.oe1.x86_64                                                 9/20 
  Running scriptlet: nginx-mod-http-xslt-filter-1:1.16.1-2.oe1.x86_64                                                 9/20 
  Installing       : nginx-mod-mail-1:1.16.1-2.oe1.x86_64                                                            10/20 
  Running scriptlet: nginx-mod-mail-1:1.16.1-2.oe1.x86_64                                                            10/20 
  Installing       : nginx-mod-stream-1:1.16.1-2.oe1.x86_64                                                          11/20 
  Running scriptlet: nginx-mod-stream-1:1.16.1-2.oe1.x86_64                                                          11/20 
  Installing       : nginx-1:1.16.1-2.oe1.x86_64                                                                     12/20 
  Running scriptlet: nginx-1:1.16.1-2.oe1.x86_64                                                                     12/20 
  Installing       : nginx-mod-http-image-filter-1:1.16.1-2.oe1.x86_64                                               13/20 
  Running scriptlet: nginx-mod-http-image-filter-1:1.16.1-2.oe1.x86_64                                               13/20 
  Installing       : nginx-all-modules-1:1.16.1-2.oe1.noarch                                                         14/20 
  Installing       : libtasn1-devel-4.13-7.oe1.x86_64                                                                15/20 
  Installing       : gmp-c++-1:6.1.2-10.oe1.x86_64                                                                   16/20 
  Running scriptlet: gmp-c++-1:6.1.2-10.oe1.x86_64                                                                   16/20 
  Installing       : gmp-devel-1:6.1.2-10.oe1.x86_64                                                                 17/20 
  Installing       : nettle-devel-3.4.1rc1-4.oe1.x86_64                                                              18/20 
  Installing       : gnutls-devel-3.6.9-5.oe1.x86_64                                                                 19/20 
  Installing       : libev-devel-4.24-11.oe1.x86_64                                                                  20/20 
  Running scriptlet: libev-devel-4.24-11.oe1.x86_64                                                                  20/20 
  Verifying        : gd-2.2.5-6.oe1.x86_64                                                                            1/20 
  Verifying        : gmp-c++-1:6.1.2-10.oe1.x86_64                                                                    2/20 
  Verifying        : gmp-devel-1:6.1.2-10.oe1.x86_64                                                                  3/20 
  Verifying        : gnutls-devel-3.6.9-5.oe1.x86_64                                                                  4/20 
  Verifying        : gperftools-libs-2.7-7.oe1.x86_64                                                                 5/20 
  Verifying        : libtasn1-devel-4.13-7.oe1.x86_64                                                                 6/20 
  Verifying        : libunwind-1.3.1-3.oe1.x86_64                                                                     7/20 
  Verifying        : libwebp-1.0.0-5.oe1.x86_64                                                                       8/20 
  Verifying        : libxslt-1.1.32-7.oe1.x86_64                                                                      9/20 
  Verifying        : nettle-devel-3.4.1rc1-4.oe1.x86_64                                                              10/20 
  Verifying        : p11-kit-devel-0.23.14-6.oe1.x86_64                                                              11/20 
  Verifying        : libev-devel-4.24-11.oe1.x86_64                                                                  12/20 
  Verifying        : nginx-1:1.16.1-2.oe1.x86_64                                                                     13/20 
  Verifying        : nginx-all-modules-1:1.16.1-2.oe1.noarch                                                         14/20 
  Verifying        : nginx-filesystem-1:1.16.1-2.oe1.noarch                                                          15/20 
  Verifying        : nginx-mod-http-image-filter-1:1.16.1-2.oe1.x86_64                                               16/20 
  Verifying        : nginx-mod-http-perl-1:1.16.1-2.oe1.x86_64                                                       17/20 
  Verifying        : nginx-mod-http-xslt-filter-1:1.16.1-2.oe1.x86_64                                                18/20 
  Verifying        : nginx-mod-mail-1:1.16.1-2.oe1.x86_64                                                            19/20 
  Verifying        : nginx-mod-stream-1:1.16.1-2.oe1.x86_64                                                          20/20 

Installed:
  gnutls-devel-3.6.9-5.oe1.x86_64                          libev-devel-4.24-11.oe1.x86_64                                  
  nginx-1:1.16.1-2.oe1.x86_64                              gd-2.2.5-6.oe1.x86_64                                           
  gmp-c++-1:6.1.2-10.oe1.x86_64                            gmp-devel-1:6.1.2-10.oe1.x86_64                                 
  gperftools-libs-2.7-7.oe1.x86_64                         libtasn1-devel-4.13-7.oe1.x86_64                                
  libunwind-1.3.1-3.oe1.x86_64                             libwebp-1.0.0-5.oe1.x86_64                                      
  libxslt-1.1.32-7.oe1.x86_64                              nettle-devel-3.4.1rc1-4.oe1.x86_64                              
  p11-kit-devel-0.23.14-6.oe1.x86_64                       nginx-all-modules-1:1.16.1-2.oe1.noarch                         
  nginx-filesystem-1:1.16.1-2.oe1.noarch                   nginx-mod-http-image-filter-1:1.16.1-2.oe1.x86_64               
  nginx-mod-http-perl-1:1.16.1-2.oe1.x86_64                nginx-mod-http-xslt-filter-1:1.16.1-2.oe1.x86_64                
  nginx-mod-mail-1:1.16.1-2.oe1.x86_64                     nginx-mod-stream-1:1.16.1-2.oe1.x86_64                          

Complete!
start nginx
nginx: [warn] could not build optimal types_hash, you should increase either types_hash_max_size: 2048 or types_hash_bucket_size: 64; ignoring types_hash_bucket_size
install nginx benchmark
Cloning into 'httpress'...
remote: Enumerating objects: 68, done.
remote: Counting objects: 100% (68/68), done.
remote: Compressing objects: 100% (32/32), done.
remote: Total 68 (delta 37), reused 66 (delta 35), pack-reused 0
Unpacking objects: 100% (68/68), done.
mkdir -p bin/Release
mkdir -p obj/Release
gcc -c -o obj/Release/httpress.o httpress.c -pthread -Wno-strict-aliasing -O2 -s -DWITH_SSL
httpress.c: In function ‘retrieve_ssl_session_info’:
httpress.c:340:3: warning: ‘gnutls_compression_get’ is deprecated [-Wdeprecated-declarations]
   conn->tdata->ssl_compression=gnutls_compression_get(session);
   ^~~~
In file included from /usr/include/gnutls/gnutls.h:3376:0,
                 from httpress.c:52:
/usr/include/gnutls/compat.h:225:1: note: declared here
 gnutls_compression_get(gnutls_session_t session) _GNUTLS_GCC_ATTR_DEPRECATED;
 ^~~~~~~~~~~~~~~~~~~~~~
httpress.c: In function ‘main’:
httpress.c:1143:9: warning: ‘gnutls_compression_get_name’ is deprecated [-Wdeprecated-declarations]
         printf ("- Compression: %s\n", gnutls_compression_get_name(tdata->ssl_compression));
         ^~~~~~
In file included from /usr/include/gnutls/gnutls.h:3376:0,
                 from httpress.c:52:
/usr/include/gnutls/compat.h:228:1: note: declared here
 gnutls_compression_get_name(gnutls_compression_method_t
 ^~~~~~~~~~~~~~~~~~~~~~~~~~~
gcc -o bin/Release/httpress obj/Release/httpress.o -lev -lpthread -lgnutls
update the nginx client
[root@openEuler nginx]#
```

​		查看一下 nginx_client.yaml 的内容：

```bash
[root@openEuler nginx]# ll
总用量 24K
drwx------. 5 root root 4.0K  1月 13 12:04 httpress
-rw-------. 1 root root  607  1月 13 11:40 nginx_benchmark.sh
-rw-------. 1 root root  292  1月 13 12:04 nginx_client.yaml
-rw-------. 1 root root  302  1月 13 12:04 nginx_http_long_client.yaml
-rw-------. 1 root root 1.3K  1月 13 11:40 prepare.sh
-rw-------. 1 root root  323  1月 13 11:40 README
[root@openEuler nginx]# cat nginx_client.yaml 
project: "nginx"
engine : "gbrt"
iterations : 30
random_starts : 10

benchmark : "sh /root/A-Tune/examples/tuning/nginx/nginx_benchmark.sh"
evaluations :
  -
    name: "rps"
    info:
        get: "echo '$out' | grep 'TIMING:' | awk '{print $4}'"
        type: "negative"
        weight: 100
[root@openEuler nginx]#
```

可以看到相关优化设置：

​		🖥 nginx的调优算法为 gbrt；

​		🖥 调优的迭代次数为 30；

​		🖥 随即迭代次数为 10；

​		🖥 benchmark执行命令为`sh /root/A-Tune/examples/tuning/nginx/nginx_benchmark.sh`；

​		🖥 性能评价指标为 ”rps“。

​		再看`nginx_benchmark.sh`的内容：

```bash
[root@openEuler nginx]# cat nginx_benchmark.sh 
#!/bin/sh
# Copyright (c) 2020 Huawei Technologies Co., Ltd.
# A-Tune is licensed under the Mulan PSL v2.
# You can use this software according to the terms and conditions of the Mulan PSL v2.
# You may obtain a copy of Mulan PSL v2 at:
#     http://license.coscl.org.cn/MulanPSL2
# THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT, MERCHANTABILITY OR FIT FOR A PARTICULAR
# PURPOSE.
# See the Mulan PSL v2 for more details.
# Create: 2020-11-26

httpress -n 1000000 -c 512 -t 7 -k http://localhost:80
```

​		该文件用来在执行调优时获取评价指标的具体数值，由上面的`nginx_client.yaml`文件中第 6 行执行该文件。

​		然后进行调优：

```bash
atune-adm tuning --project <PROJECT_NAME> --detail <client.yaml>    # <PROJECT_NAME>为调优实例名；client.yaml保存了客户端调优的评价指标等信息
```



```bash
[root@openEuler nginx]# atune-adm tuning --project nginx --detail nginx_client.yaml
 Start to benchmark baseline...
 1.Loading its corresponding tuning project: nginx
 2.Start to tuning the system......
 Current Tuning Progress......(1/30)
 Used time: 1m0s, Total Time: 1m0s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 1th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 1th evaluation value: (rps=36537.00)(11.19%)
 Current Tuning Progress......(2/30)
 Used time: 1m29s, Total Time: 1m29s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 2th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 2th evaluation value: (rps=36401.00)(10.78%)
 Current Tuning Progress......(3/30)
 Used time: 1m59s, Total Time: 1m59s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 3th recommand parameters is: nginx.access_log=off,nginx.error_log=/dev/null
 The 3th evaluation value: (rps=36400.00)(10.77%)
 Current Tuning Progress......(4/30)
 Used time: 2m30s, Total Time: 2m30s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 4th recommand parameters is: nginx.access_log=/var/log/nginx/access.log  main,nginx.error_log=/dev/null
 The 4th evaluation value: (rps=34112.00)(3.81%)
 Current Tuning Progress......(5/30)
 Used time: 3m2s, Total Time: 3m2s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 5th recommand parameters is: nginx.access_log=/var/log/nginx/access.log  main,nginx.error_log=/dev/null
 The 5th evaluation value: (rps=34223.00)(4.15%)
 Current Tuning Progress......(6/30)
 Used time: 3m32s, Total Time: 3m32s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 6th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 6th evaluation value: (rps=35948.00)(9.40%)
 Current Tuning Progress......(7/30)
 Used time: 4m2s, Total Time: 4m2s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 7th recommand parameters is: nginx.access_log=off,nginx.error_log=/dev/null
 The 7th evaluation value: (rps=35564.00)(8.23%)
 Current Tuning Progress......(8/30)
 Used time: 4m34s, Total Time: 4m34s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 8th recommand parameters is: nginx.access_log=/var/log/nginx/access.log  main,nginx.error_log=/var/log/nginx/error.log
 The 8th evaluation value: (rps=33251.00)(1.19%)
 Current Tuning Progress......(9/30)
 Used time: 5m4s, Total Time: 5m4s, Best Performance: (rps=36537.00), Performance Improvement Rate: 11.19%
 The 9th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 9th evaluation value: (rps=35572.00)(8.25%)
 Current Tuning Progress......(10/30)
 Used time: 5m33s, Total Time: 5m33s, Best Performance: (rps=36744.00), Performance Improvement Rate: 11.82%
 The 10th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 10th evaluation value: (rps=36744.00)(11.82%)
 Current Tuning Progress......(11/30)
 Used time: 6m3s, Total Time: 6m3s, Best Performance: (rps=36744.00), Performance Improvement Rate: 11.82%
 The 11th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 11th evaluation value: (rps=36334.00)(10.57%)
 Current Tuning Progress......(12/30)
 Used time: 6m33s, Total Time: 6m33s, Best Performance: (rps=36744.00), Performance Improvement Rate: 11.82%
 The 12th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 12th evaluation value: (rps=36315.00)(10.51%)
 Current Tuning Progress......(13/30)
 Used time: 7m1s, Total Time: 7m1s, Best Performance: (rps=38889.00), Performance Improvement Rate: 18.35%
 The 13th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 13th evaluation value: (rps=38889.00)(18.35%)
 Current Tuning Progress......(14/30)
 Used time: 7m31s, Total Time: 7m31s, Best Performance: (rps=38889.00), Performance Improvement Rate: 18.35%
 The 14th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 14th evaluation value: (rps=35704.00)(8.65%)
 Current Tuning Progress......(15/30)
 Used time: 7m58s, Total Time: 7m58s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 15th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 15th evaluation value: (rps=39483.00)(20.16%)
 Current Tuning Progress......(16/30)
 Used time: 8m26s, Total Time: 8m26s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 16th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 16th evaluation value: (rps=38853.00)(18.24%)
 Current Tuning Progress......(17/30)
 Used time: 8m54s, Total Time: 8m54s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 17th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 17th evaluation value: (rps=38714.00)(17.81%)
 Current Tuning Progress......(18/30)
 Used time: 9m23s, Total Time: 9m23s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 18th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 18th evaluation value: (rps=37297.00)(13.50%)
 Current Tuning Progress......(19/30)
 Used time: 9m52s, Total Time: 9m52s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 19th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 19th evaluation value: (rps=37886.00)(15.30%)
 Current Tuning Progress......(20/30)
 Used time: 10m21s, Total Time: 10m21s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 20th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 20th evaluation value: (rps=36705.00)(11.70%)
 Current Tuning Progress......(21/30)
 Used time: 10m51s, Total Time: 10m51s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 21th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 21th evaluation value: (rps=36459.00)(10.95%)
 Current Tuning Progress......(22/30)
 Used time: 11m19s, Total Time: 11m19s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 22th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 22th evaluation value: (rps=38114.00)(15.99%)
 Current Tuning Progress......(23/30)
 Used time: 11m50s, Total Time: 11m50s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 23th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 23th evaluation value: (rps=35049.00)(6.66%)
 Current Tuning Progress......(24/30)
 Used time: 12m19s, Total Time: 12m19s, Best Performance: (rps=39483.00), Performance Improvement Rate: 20.16%
 The 24th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 24th evaluation value: (rps=37418.00)(13.87%)
 Current Tuning Progress......(25/30)
 Used time: 12m46s, Total Time: 12m46s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 25th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 25th evaluation value: (rps=40134.00)(22.14%)
 Current Tuning Progress......(26/30)
 Used time: 13m16s, Total Time: 13m16s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 26th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 26th evaluation value: (rps=36342.00)(10.60%)
 Current Tuning Progress......(27/30)
 Used time: 13m44s, Total Time: 13m44s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 27th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 27th evaluation value: (rps=38562.00)(17.35%)
 Current Tuning Progress......(28/30)
 Used time: 14m12s, Total Time: 14m12s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 28th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 28th evaluation value: (rps=38593.00)(17.45%)
 Current Tuning Progress......(29/30)
 Used time: 14m42s, Total Time: 14m42s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 29th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 29th evaluation value: (rps=35933.00)(9.35%)
 Current Tuning Progress......(30/30)
 Used time: 15m13s, Total Time: 15m13s, Best Performance: (rps=40134.00), Performance Improvement Rate: 22.14%
 The 30th recommand parameters is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The 30th evaluation value: (rps=35283.00)(7.37%)
 
 The final optimization result is: nginx.access_log=off,nginx.error_log=/var/log/nginx/error.log
 The final evaluation value is: rps=40134.00

 Baseline Performance is: (rps=32860.00)
 
 Tuning Finished
[root@openEuler nginx]#
```

​		一般调优大概10分钟左右，注意最后几行告诉我们错误日志信息保存在`/dev/null`，rps 由最初的`32860.00`增长到`40134.00`。

​		`RPS`：全名为`Requests per second`，表示 nginx 每秒处理的请求数，以此作为 nginx 的性能指标