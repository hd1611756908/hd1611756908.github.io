---
title: Atlas中间件快速入门(实现数据库读写分离)
categories: MYSQL
tags: ["MySQL","Atlas","读写分离","负载均衡"]
---

```
Author: 夜泊
Email : hd1611756908@163.com
QQ    : 1611756908
web   : https://ukoko.gitee.io
sign  : 每走一步,都是进步
```

## 一、Atlas简介
```
1. Atlas是由 Qihoo 360公司Web平台部基础架构团队开发维护的一个基于MySQL协议的数据中间层项目
2. 它在MySQL官方推出的MySQL-Proxy 0.8.2版本的基础上，修改了大量bug，添加了很多功能特性
3. 目前该项目在360公司内部得到了广泛应用，很多MySQL业务已经接入了Atlas平台，每天承载的读写请求数达几十亿条
4. 有超过50家公司在生产环境中部署了Atlas，超过800人已加入了我们的开发者交流群，并且这些数字还在不断增加
```
> 主要功能

* 1.读写分离
* 2.从库负载均衡
* 3.IP过滤
* 4.自动分表
* 5.DBA可平滑上下线DB
* 6.自动摘除宕机的DB

---

<!--more-->

## 二、MySQL主从系统搭建

> 设计图

<img src="https://note.youdao.com/yws/api/personal/file/9FBF8C4B0EB644A68A3866B82B6A5C3D?method=download&shareKey=2bd0311a0a09f57ef617d9bafad70ca8">

> 介绍

```
由图可以看出要想使用Atlas首先我们需要搭建一个MySQL的集群,Atlas作为数据库中间件,可以帮助我们进行请求路由,负载均衡等
```

> MySQL主从复制搭建

* MySQL服务器版本选择
```
Atlas官网安装的注意事项中提到MySQL应该选择的版本(建议大于5.1版本,最好使用5.6版本)

我这里使用5.6版本进行演示.
```
* 搭建步骤
* * 注意事项
```
我这里搭建MySQL集群使用docker容器进行搭建
1. 没有那么多硬件资源
2. 搭建方便
```
* * 搭建步骤

> 1. 本机要安装docker如果没有安装请移步这里进行安装

```
https://ukoko.gitee.io/2021/03/05/%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8docker/
```
> 2. 拉取MySQL5.6版本的镜像

```
1. 进入duckerhub网站
   https://hub.docker.com/
2. 搜索mysql
3. 查看mysql版本,并且5.6版本的mysql拉取到本地
   拉取命令 docker pull mysql:5.6.51
```

> 3. 开始进行安装

```
1. 首先在docker平台上启动两个MySQL服务,一个作为主,一个作为从
2. 启动MySQL
   主(名字叫mysql-master)
      --> docker run -p3307:3306 --name mysql-master --privileged=true -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6.51
   从(名字叫mysql-slave)
      --> docker run -p3308:3306 --name mysql-slave --privileged=true -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6.51
3. 启动完成之后,使用docker ps命令查看当前运行的mysql容器

[root@localhost ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                    NAMES
329f0925de00   mysql:5.6.51   "docker-entrypoint.s…"   9 seconds ago    Up 7 seconds    0.0.0.0:3308->3306/tcp   mysql-slave
b6e89b46286f   mysql:5.6.51   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds   0.0.0.0:3307->3306/tcp   mysql-master

4. 设置mysql-master为主,设置mysql-slave为从

   4.1 使用docker命令进入mysql-master容器内部
       
       docker exec -it b6e89b46286f bash
       进入容器之后因为容器内部没有安装vim编辑器,所以首先安装vim编辑器
       在安装vim之前首先更新一个apt库 所以先执行更新库命令  apt-get update
       更新库完成之后在执行安装vim编辑器命令 apt-get install -y vim
       如果不先更新apt库,那么vim可能不会安装成功
       
       使用vim编辑器打开mysql的配置文件my.cnf  vim /etc/mysql/my.cnf
       在my.cnf配置文件中首先就是设置server-id=100 设置一个任意整数即可,是MySQL集群进行编号.
       在my.cnf中开启log-bin日志(很重要,MySQL集群,数据同步的关键) log-bin=mysql-bin
       
       配置如下:
       
       [mysqld]
       # MySQL集群编号,不能重复
       server-id=100 
       # 开启二进制日志功能,日志名字随便取
       log-bin=mysql-bin
       
       在当前的MySQL主服务器中创建用户,从服务器可以用新创建的用户进行数据同步(也可以直接使用root用户)
       我这里不重新创建用户了,直接使用root用户进行数据同步,但是需要将root用户设置成可以进行数据同步
       GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'root'@'%';
       刷新权限  FLUSH PRIVILEGES;
       
       到此为止,主服务器配置完成(配置完成之后最好是重启mysql服务器)
       
       查看主服务器的状态在MySQL的交互界面输入 show master status;
       
       mysql> show master status;
       +------------------+----------+--------------+------------------+-------------------+
       | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
       +------------------+----------+--------------+------------------+-------------------+
       | mysql-bin.000001 |      360 |              |                  |                   |
       +------------------+----------+--------------+------------------+-------------------+
       1 row in set (0.00 sec)
       
   4.2 使用docker命令进入mysql-slave容器内部

       docker exec -it 329f0925de00 bash
       
       他的内部也没有vim所以和主节点安装方式一样,安装vim编辑器
       使用vim编辑器打开mysql-slave的配置文件my.cnf  vim /etc/mysql/my.cnf
       
       配置如下:
       
       [mysqld]
       # MySQL集群编号,不能重复
       server-id=101
       # 中继日志
       relay_log=edu-mysql-relay-bin

       配置完从容器之后,重启从容器,然后计入到容器中,使用myql -uroot -p登陆客户端
       
    4.3 两台容器都配置完成,接下来开始将两个容器关联起来.
        
       4.3.1 分别进入两个容器中使用mysql -uroot -p 登陆进mysql交互环境
       4.3.2 在主服务器中使用命令查看当前master服务器状态 show master status; 记录下File和Position设置从服务器时需要用到这两个数据
       4.3.3 在从服务器中使用命令关联主服务器,这时需要知道主服务器的IP地址可以使用docker命令查询主服务器的IP地址
       查询IP地址 docker inspect --format='{{.NetworkSettings.IPAddress}}' mysql-master  (这条命令是docker命令不能在MySQL交互环境执行,需要在容器外面执行)
       
       4.3.4 执行了主服务器的IP地址之后在从服务器的MySQL交互页面开始关联主服务器
       
       change master to master_host='172.17.0.2', \
       master_user='root',\
       master_password='123456', \
       master_port=3306, \
       master_log_file='mysql-bin.000001', \
       master_log_pos= 360, \
       master_connect_retry=30;
       
       配置说明：

       master_host：Master的地址
       master_port：Master的端口号，指的是容器的端口号
       master_user：用于数据同步的用户
       master_password：用于同步的用户的密码
       master_log_file：指定 Slave 从哪个日志文件开始复制数据，即上文中提到的 File 字段的值
       master_log_pos：从哪个 Position 开始读，即上文中提到的 Position 字段的值
       master_connect_retry：如果连接失败，重试的时间间隔，单位是秒，默认是60秒
       4.3.5 开启slave    start slave;
       4.3.6 在从服务器中查看主从同步状态  show slave status \G;
       
       mysql> show slave status \G;
        *************************** 1. row ***************************
                       Slave_IO_State: Waiting for master to send event
                          Master_Host: 172.17.0.2
                          Master_User: root
                          Master_Port: 3306
                        Connect_Retry: 30
                      Master_Log_File: mysql-bin.000001
                  Read_Master_Log_Pos: 360
                       Relay_Log_File: edu-mysql-relay-bin.000002
                        Relay_Log_Pos: 283
                Relay_Master_Log_File: mysql-bin.000001
                     Slave_IO_Running: Yes    # YES为成功
                    Slave_SQL_Running: Yes    # YES为成功
                      Replicate_Do_DB:
                  Replicate_Ignore_DB:
                   Replicate_Do_Table:
               Replicate_Ignore_Table:
              Replicate_Wild_Do_Table:
          Replicate_Wild_Ignore_Table:
                           Last_Errno: 0
                           Last_Error:
                         Skip_Counter: 0
                  Exec_Master_Log_Pos: 360
                      Relay_Log_Space: 460
                      Until_Condition: None
                       Until_Log_File:
                        Until_Log_Pos: 0
                   Master_SSL_Allowed: No
                   Master_SSL_CA_File:
                   Master_SSL_CA_Path:
                      Master_SSL_Cert:
                    Master_SSL_Cipher:
                       Master_SSL_Key:
                Seconds_Behind_Master: 0
        Master_SSL_Verify_Server_Cert: No
                        Last_IO_Errno: 0
                        Last_IO_Error:
                       Last_SQL_Errno: 0
                       Last_SQL_Error:
          Replicate_Ignore_Server_Ids:
                     Master_Server_Id: 100
                          Master_UUID: 8c7e9e61-a265-11eb-907c-0242ac110002
                     Master_Info_File: /var/lib/mysql/master.info
                            SQL_Delay: 0
                  SQL_Remaining_Delay: NULL
              Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
                   Master_Retry_Count: 86400
                          Master_Bind:
              Last_IO_Error_Timestamp:
             Last_SQL_Error_Timestamp:
                       Master_SSL_Crl:
                   Master_SSL_Crlpath:
                   Retrieved_Gtid_Set:
                    Executed_Gtid_Set:
                        Auto_Position: 0
        1 row in set (0.00 sec)

```

> 4. 测试安装是否成功

```
测试主从同步是否成功

在服务器上创建一个数据库,在从服务器上查看
在主服务器新创建的库中新创建一张表,在从服务器中查看
在主服务器中插入一条数据,在从服务器中查看

注意: 由于我们使用的root账户进行操作,这时候从数据库也是可以进行数据修改的,但是从数据库不会将数据同步到主数据库,你可以设置将从数据库设置成只读,这里不做介绍.
```

> 以上MySQL的主从架构就设计完成了.接下来就是配置中间件Atlas

---
## 三、Atlas的安装/配置

* 官网地址
```
https://github.com/Qihoo360/Atlas/blob/master/README_ZH.md
```
* 下载地址
```
https://github.com/Qihoo360/Atlas/releases/download/2.2.1/Atlas-2.2.1.el6.x86_64.rpm
```
* 安装注意事项
```
1. Atlas只能安装在64位系统上
2. Centos 5.X安装 Atlas-XX.el5.x86_64.rpm，Centos 6.X安装Atlas-XX.el6.x86_64.rpm
3. 我在Centos7.x上面安装Atlas-XX.el6.x86_64.rpm也没问题
4. 如果本机安装过,那么需要先卸载
5. mysql版本应大于5.1,建议使用Mysql 5.6
```
* 安装步骤
```
Atlas安装及其简单只需要一条命令很快安装完成
sudo rpm –i Atlas-XX.el6.x86_64.rpm
安装目录在/usr/local/mysql-proxy下
安装好之后就是配置介绍
```
* 配置文件(/usr/local/mysql-proxy/conf/test.cnf)

> 里面介绍了一些非必须的配置,我这里将非必须的都删除掉,如果有需要请自行查看

```
[mysql-proxy]

#管理接口的用户名(默认即可,进入管理客户端的用户名,非操作数据库的用户名)
admin-username = user

#管理接口的密码(默认即可)
admin-password = pwd

#Atlas后端连接的MySQL主库的IP和端口，可设置多项，用逗号分隔
proxy-backend-addresses =192.168.1.134:3307

#Atlas后端连接的MySQL从库的IP和端口，@后面的数字代表权重，用来作负载均衡，若省略则默认为1，可设置多项，用逗号分隔
#proxy-read-only-backend-addresses = 192.168.1.134:3308@1

#用户名与其对应的加密过的MySQL密码，密码使用PREFIX/bin目录下的加密程序encrypt加密，下行的root为示例，将其替换为你的MySQL的用户名和加密密码！
pwds = root:/iZxz+0GRoA=

#设置Atlas的运行方式，设为true时为守护进程方式，设为false时为前台方式，一般开发调试时设为false，线上运行时设为true,true后面不能有空格。
daemon = true

#设置Atlas的运行方式，设为true时Atlas会启动两个进程，一个为monitor，一个为worker，monitor在worker意外退出后会自动将其重启，设为false时只有worker，没有monitor，一般开发调试时设为false，线上运行时设为true,true后面不能有空格。
keepalive = true

#工作线程数，对Atlas的性能有很大影响，可根据情况适当设置
event-threads = 2

#日志级别，分为message、warning、critical、error、debug五个级别
log-level = message

#日志存放的路径
log-path = /usr/local/mysql-proxy/log

#实例名称，用于同一台机器上多个Atlas实例间的区分
#instance = test

#Atlas监听的工作接口IP和端口,相当于MySQL的3306端口,用于操作数据库的,以前我们都是直连数据库,现在我们连接中间件即可.
proxy-address = 0.0.0.0:1234

#Atlas监听的管理接口IP和端口
admin-address = 0.0.0.0:2345
```

> 上面就是Atlas的最简配置,如果有什么特殊需要请查看详细官网,地址为

```
https://github.com/Qihoo360/Atlas/wiki/Atlas%E7%9A%84%E5%AE%89%E8%A3%85
```

---
## 四、Atlas的启动和测试

* 启动
```
进入/usr/local/mysql-proxy/bin目录,执行下面的命令启动、重启或停止Atlas.
(1). sudo ./mysql-proxyd test start   启动Atlas。
(2). sudo ./mysql-proxyd test restart 重启Atlas。
(3). sudo ./mysql-proxyd test stop    停止Atlas。

注意:
1. 运行文件是：mysql-proxyd(不是mysql-proxy)
2. test是conf目录下配置文件的名字，也是配置文件里instance项的名字，三者需要统一
3. 可以使用ps -ef | grep mysql-proxy查看Atlas是否已经启动或停止
```
* 测试

> 测试1234端口号,是否能操作数据库

```
mysql -uroot -h192.168.1.134 -P1234 -p123456
```

> 测试2345端口号,是否能进入管理控制台,对MySQL集群进行管理

```
mysql -uuser -h192.168.1.134 -P2345 -ppwd

通过select * from help;查看管理DB的各类命令

1. SELECT * FROM backends; 查看当前MySQL的服务节点列表
2. SELECT * FROM pwds;     查看MySQL加密之后的密码
3. SELECT VERSION;         查看当前Atlas版本
```

> 以上为Atlas的设计,如果想了解其他的比如说高可用,可以查看官网地址为:

```
https://github.com/Qihoo360/Atlas/wiki/Atlas%E7%9A%84%E6%9E%B6%E6%9E%84
```
---
## 五、MyBatis连接Atlas操作数据库
```
略,和操作单点数据库一模一样

注意: 由于MySQL数据库版本较低,所以对应MySQL的驱动尽量做到兼容,替换成5.x
```