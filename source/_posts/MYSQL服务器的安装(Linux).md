---
title: MySQL服务器安装(Linux)
categories: MYSQL
tags: ["Linux","MySQL","Centos7","HA","MySQL集群","MySQL InnoDB Cluster","mysql shell","mysql router"]
---
```
Author: 夜泊1990
Email : hd1611756908@163.com
QQ    : 1611756908
web   : https://ukoko.gitee.io
sign  : 每走一步,都是进步
```
---

> 如果网址登陆不上去,可以发邮件或者企鹅号联系我

---

## 第一章 MySQL单点安装
```
此笔记讲解的是MySQL在Linux服务器(Centos7.x)上面的安装教程
```

<!-- more -->

### 第1节 系统需求
```
在Linux服务器上安装之前首先要对Linux服务器做一些设置
1. 保证Linux服务器的防火墙是关闭的,或者是设置3306端口号可以穿透此机器的防火墙
   1.1 查看防火墙状态        firewall-cmd --state    
   1.2 关闭防火墙            service firewalld stop
   1.3 禁用防火墙开机启动    systemctl disable firewalld.service
    
2. 保证此Linux服务器的网络命令比如ifconfig是可以使用的
   2.1 安装 net-tools.x86_64  yum -y install net-tools.x86_64
   
3. 我这里要向此服务器上传文件,所以需要此服务器支持上传
   3.1 安装 lrzsz             yum -y install lrzsz
  
  由于我使用的Centos7 mini版本所以很多命令都需要手动安装,如果你的操作系统有上面这些命令,可以略过.
```

---

### 第2节 MYSQL下载
```
对于程序员来下载软件尽量去官网,我们使用的软件大多都是开源免费的,如果在网络上随便下载,可能软件里被集成了其他的东西所以慎重.

我这里下载的是MYSQL 8.0.23 版本(Linux通用安装版)
通用安装版的好处是这个安装包在任意的Linux发行版的系统上都可以安装.
```
* 下载地址
```
https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.23-linux-glibc2.12-x86_64.tar.xz
```

---

### 第3节 MYSQL安装
* 官网安装文档
```
https://dev.mysql.com/doc/refman/8.0/en/binary-installation.html
```
* 安装前的准备
```
1. 根据官网文档,如果本机以前安装过MYSQL需要卸载
2. 如果没有安装过MYSQL,我的当前的版本Linux在/etc文件夹下也会有一个my.cnf的mysql配置文件,需要删掉(当前etc文件夹下关于mysql的都需要删掉)
3. MYSQL安装需要依赖libaio库,所以需要安装这个库  yum install libaio

注意:如果在Oracle Linux 8和Red Hat 8这两个系统上面安装MYSQL,需要安装ncurses-compat-libs库(yum install ncurses-compat-libs)
```
* 安装
```
注意: 严格按照下面步骤来,不允许任意修改安装路径以及文件的命名方式

1. 创建MYSQL服务器运行的用户名和群组(MYSQL服务器运行需要自己的用户名和群组而非root)

   shell> groupadd mysql
   shell> useradd -r -g mysql -s /bin/false mysql

2. 将下载的mysql压缩包复制到/usr/local文件夹下(Linux的默认安装路径(官网指定),有点像我们window的Program Files)

   shell> cp 源文件 目标目录
   shell> cp /home/hs/mysql-8.0.23-linux-glibc2.12-x86_64.tar.xz  /usr/local/
   
3. 将/usr/local下的mysql压缩包解压(压缩包后缀为xz)
   
   shell> tar -xvJf mysql-8.0.23-linux-glibc2.12-x86_64.tar.xz

4. 将解压之后的文件夹重命名为mysql(如果不修改可以试试,后面会不会报错,如果报错仔细读一下错误消息)

   shell> mv mysql-8.0.23-linux-glibc2.12-x86_64 mysql

5. 在mysql文件夹下创建mysql-files文件夹

   shell> mkdir mysql-files

6. 使用chown命令修改mysql-files文件的拥有者

   shell> chown mysql:mysql mysql-files

7. 使用chmod命令修改mysql-files文件的权限

   shell> chmod 750 mysql-files

8. 初始化安装(这里会生成一个初始化密码,记住)

   shell> bin/mysqld --initialize --user=mysql

9. 生成SSL和RSA文件(提高通信安全)

   shell> bin/mysql_ssl_rsa_setup

10. 启动mysql服务

   shell> bin/mysqld_safe --user=mysql &
   
   MYSQL启动之后使用 ps -ef | grep mysql 命令查看是否启动成功,如果启动成功,可以使用mysql命令登陆
   
   shell> bin/mysql -uroot -p   密码在第8步的初始化安装的时候生成
   
   使用命令登陆成功之后,MYSQL服务会提示你重新修改密码,修改命令为
   mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';

11. 设置开机启动(在Linux服务器重启时,MYSQL服务器会自动开机启动)
   
   11.1 第1步要将support-files文件夹下的mysql.server文件复制到/etc/init.d目录下
        shell> cp support-files/mysql.server /etc/init.d/mysql.server
   11.2 第2步查看/etc/init.d/mysql.server文件有没有执行权限,如果没有需要添加执行权限
        shell> chmod +x /etc/init.d/mysql.server
   11.3 将mysql启动脚本加入到服务列表
        shell> chkconfig --add mysql.server
   11.4 查看是否mysql启动脚本是否添加成功
        shell> chkconfig --list
        
   如果显示类似于以下的列表为成功(有可能是英文)
   
       注：该输出结果只显示 SysV 服务，并不包含
    原生 systemd 服务。SysV 配置数据
    可能被原生 systemd 配置覆盖。 
    
          要列出 systemd 服务，请执行 'systemctl list-unit-files'。
          查看在具体 target 启用的服务请执行
          'systemctl list-dependencies [target]'。
    
    mysql.server   	0:关	1:关	2:开	3:开	4:开	5:开	6:关
    netconsole     	0:关	1:关	2:关	3:关	4:关	5:关	6:关
    network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关

   ------------------------------------------------------------------------------

   注意:  mysql.server这一行的3,4,5如果为开(英文为ON),说明开机启动设置成功,否则使用命令设置
   
   shell> chkconfig --level 345 mysql on
   
12. 重新启动Linux服务器验证是否成功

   shell> reboot

13. 修改root用户的host值

   root用户的的host值默认为localhost也就是说除了127.0.0.1和localhost可以访问当前数据库,其他的地址都不能访问,所以修改root用户的host为%,就可以任意主机访问了
   
14. 如果有必要可以设置MYSQL的环境变量,这样操作起来更方便(不重要)
```
---
## 第二章 MYSQL集群安装(HA)
```
MYSQL的官网提供了HA(高可用)的解决方案
 
1. MySQL InnoDB Cluster集群(主要)
2. MySQL NDB Cluster集群

这里只介绍MySQL InnoDB Cluster集群的生产环境的安装方式,不阐述官网说的沙箱方式
```

---

### 第1节 系统需求

* MYSQL InnoDB Cluster协同原理图

<center>

<img src="https://note.youdao.com/yws/api/personal/file/D36615E132DB496E8C0028A8BEC4E04C?method=download&shareKey=59bae1354ce5344496bf79a77e61cb63">

</center>


* 协同原理图介绍
```
上图为官网提供的协同工作原理图,由图可以看出里面共有6个服务节点(每一个方块都可以看成一台服务器)

1. Client App(MYSQL Connector),这个节点部署的是项目(非重点,和本章课程无关,可以看成是操作数据库的Java项目)

2. MySQL Shell(Cluster Admin) MYSQL客户端,用于管理和配置集群(重要)

3. MySQL Router节点,主要进行请求路由,将客户端发送过来的请求路由到不同的MYSQL服务器节点(重要)

4. MySQL Servers里面有三个服务节点(官网指定服务的节点数最少3个最多9个),如果过多或造成性能下降
   4.1 其中有一个主节点,两个从节点
   4.2 主节点可以进行读写操作,从节点只能进行读取操作
   4.3 集群设置默认为单主模式,当然还可以设置成多主模式
```
* MySQL InnoDB Cluster安装前需求
```
1. 根据原理图我们需要准备5台Linux服务器
   1.1 3个MySQL实例服务(3台)
   1.2 MySQL Shell 服务(1台)
   1.3 MySQL Router服务(1台)   //节约硬件资源,我这里将MySQL Shell和MySQL Router安装在一台服务器上面.
2. 首先就是准备3台服务器,在每一台服务器上面都需要安装MySQL服务,安装方式看第一章MySQL单点安装
   2.1 安装好MySQL服务器之后需要做一些设置
      -> 设置MySQL的root账户的初始化密码(保证三个机器的密码设置成相同的)
      -> 设置root账户的host为%,这样就可以任意其他机器都可以访问
      -> 设置Linux服务器的防火墙(关闭并且禁用)
3. 禁用SELinux 
   3.1 查看当前SELinux的状态   命令为: sestatus
   3.2 禁用方式,修改/etc/selinux/config里面的SELINUX=disabled即可
   
以上就是配置前的基础准备工作(重启一下服务器)
```
---
### 第2节 MySQL Shell介绍
```
MySQL集群的管理客户端,用户对MySQL集群的配置和管理
```
* 下载地址
```
https://dev.mysql.com/get/Downloads/MySQL-Shell/mysql-shell-8.0.23-linux-glibc2.12-x86-64bit.tar.gz
```

---

### 第3节 MySQL Shell安装

* 安装需求

```
1. MySQL Shell支持 JavaScript,Python,SQL脚本,在8.0.18版本之前必须保证当前的Linux操作系统安装了Python 2.7,如果没有需要自行安装

2. 但是在MySQL Shell 8.0.18 版本之后开始使用Python3而不是Python2.7了,不过也不再需要操作系统集成了,因为他的安装包自身已经捆绑了Python3.7.7
```

* 安装/启动步骤
```
1. 将下载的压缩包复制到/usr/local目录下(个人习惯将文件安装到这个目录)

   shell> cp /home/hs/mysql-shell-8.0.23-linux-glibc2.12-x86-64bit.tar.gz /usr/local/
   
2. 解压mysql-shell-8.0.23-linux-glibc2.12-x86-64bit.tar.gz
   
   shell> tar -zxvf mysql-shell-8.0.23-linux-glibc2.12-x86-64bit.tar.gz
   
3. 将解压之后的mysql-shell-8.0.23-linux-glibc2.12-x86-64bit重命名为mysql-shell

   shell> mv mysql-shell-8.0.23-linux-glibc2.12-x86-64bit mysql-shell
   
4. 启动命令在mysql-shell/bin目录下
   
   4.1 启动:
   
    shell> /usr/local/mysql-shell/bin/mysqlsh
    
   4.2 设置环境变量方便操作(将下面的这句话设置到/etc/profile文件最下面即可)
    export PATH=/usr/local/mysql-shell/bin:$PATH
    
    设置完成环境变量需要使用命令让配置生效 source /etc/profile


以上 MySQL Shell安装完成
```

---

### 第4节 MySQL Shell的常用命令/函数

#### 4.1 连接MySQL服务器

* 方式一
```
1. 直接在命令行打mysqlsh命令进入mysql-shell交互模式(默认是JavaScript交互模式,可以切换模式)
   shell> mysqlsh

2. 在交互模式中使用 shell.connect('root@192.168.1.131:3306') 连接MySQL服务器
    
    2.1 shell   : JS的核心对象
    2.2 connect : 连接方法,参数是MySQL服务器的地址
    
    注意事项:
        1. 如果MySQL服务器的主机防火墙没有关闭,连接失败
        2. 如果MySQL的root用户的host没有设置成%,连接失败
   
    连接时会出现类似于下面的样式,因为连接MySQL需要输入MySQL服务器的密码
   
    MySQL  JS > shell.connect('root@192.168.1.131:3306')
    Creating a session to 'root@192.168.1.131:3306'
    Please provide the password for 'root@192.168.1.131:3306': ****
   
   
   连接成功之后会出现类似于下面的交互模式,可以看出默认也是JS交互模式
   
    MySQL  192.168.1.131:3306 ssl  JS >

3. 切换交互模式
    3.1 MySQL Shell提供了三种脚本语言的交互模式 JS、Pyhton、SQL
    3.2 切换方式使用斜线加上语言所写
    
    eg: 切换到python模式
    MySQL  192.168.1.131:3306 ssl  JS > \py
    
    切换后
    
    Switching to Python mode...
    MySQL  192.168.1.131:3306 ssl  Py >
    
    -----------------------------------------------------------------------------
    eg: 切换到SQL
    MySQL  192.168.1.131:3306 ssl  Py > \sql
    
    切换后
    
    Switching to SQL mode... Commands end with ;
    MySQL  192.168.1.131:3306 ssl  SQL >
    
    -----------------------------------------------------------------------------
    eg: 切换到JS
    MySQL  192.168.1.131:3306 ssl  SQL > \js
    
    切换后
    
    Switching to JavaScript mode...
    MySQL  192.168.1.131:3306 ssl  JS >
```
* 方式二
```
采用直接连接的方式,如果想要连接那个MySQL实例直接使用命令而不是需要shell.connect()函数

shell> mysqlsh -h192.168.1.131 -P3306 -uroot -p
Please provide the password for 'root@192.168.1.131:3306': ****(输入密码)

就会直接连接上MySQL服务器
MySQL  192.168.1.131:3306 ssl  JS >
```
* 方式三

```
MySQL  JS > \c "root@192.168.1.131:3306" 

\c: c是连接的英文单词首字母,这样的方式也是可以的,会一种即可

```

* 退出交互模式
```
方式一
MySQL  192.168.1.131:3306 ssl  JS > \q

方式二
MySQL  192.168.1.131:3306 ssl  JS > \quit
```
---

#### 4.2 操作数据库(库、表、数据)

```
操作MySQL数据库需要进去SQL交互模式,进入之后正常使用SQL语言即可,这里不做介绍
```
---

#### 4.3 检查MySQL服务器是否符合设置集群的标准
```
我这里的校验在JS的交互模式下进行
```
```
MySQL  192.168.1.131:3306 ssl  JS > dba.checkInstanceConfiguration()



注意: 我这里是用root用户作为测试,在执行完上面的检查函数之后,会报我当前登陆的用户权限不够,需要添加权限.报错信息如下:


MySQL  192.168.1.100:3306 ssl  JS > dba.checkInstanceConfiguration()
Validating MySQL instance at localhost.localdomain:3306 for use in an InnoDB cluster...
ERROR: The account 'root'@'%' is missing privileges required to manage an InnoDB cluster:
GRANT BACKUP_ADMIN, CLONE_ADMIN, PERSIST_RO_VARIABLES_ADMIN, REPLICATION_APPLIER, SYSTEM_VARIABLES_ADMIN ON *.* TO 'root'@'%' WITH GRANT OPTION;
For more information, see the online documentation.
Dba.checkInstanceConfiguration: The account 'root'@'%' is missing privileges required to manage an InnoDB cluster. (RuntimeError)


上面就是报错信息,我这里为了测试方便直接就给当前用户所有操作权限

命令为: 在mysql的交互模式下输入(不是在MySQL Shell模式)
GRANT ALL ON *.* TO 'root'@'%' WITH GRANT OPTION;

执行完上面的命令之后,需要刷新MySQL服务的权限,可以重启MySQL服务器,或者是使用MySQL命令刷新


权限设置完成之后,重新执行MySQL服务器检查命令

MySQL  192.168.1.100:3306 ssl  JS > dba.checkInstanceConfiguration()

```

* 如果MySQL服务器满足要求会打印出类似于下面这样的文字

```
Validating MySQL instance at node1:3306 for use in an InnoDB cluster...

This instance reports its own address as node1:3306
Clients and other cluster members will communicate with it through this address by default. If this is not correct, the report_host MySQL system variable should be changed.

Checking whether existing tables comply with Group Replication requirements...
No incompatible tables detected

Checking instance configuration...
Instance configuration is compatible with InnoDB cluster

The instance 'node1:3306' is valid to be used in an InnoDB cluster.

{
    "status": "ok"
}
```

* 如果MySQL服务器不满足要求会打印出类似于下面这样的文字

```
MySQL  192.168.1.100:3306 ssl  JS > dba.checkInstanceConfiguration()
Validating MySQL instance at localhost.localdomain:3306 for use in an InnoDB cluster...

This instance reports its own address as localhost.localdomain:3306
Clients and other cluster members will communicate with it through this address by default. If this is not correct, the report_host MySQL system variable should be changed.

Checking whether existing tables comply with Group Replication requirements...
No incompatible tables detected

Checking instance configuration...

NOTE: Some configuration options need to be fixed:
+----------------------------------------+---------------+----------------+--------------------------------------------------+
| Variable                               | Current Value | Required Value | Note                                             |
+----------------------------------------+---------------+----------------+--------------------------------------------------+
| binlog_transaction_dependency_tracking | COMMIT_ORDER  | WRITESET       | Update the server variable                       |
| enforce_gtid_consistency               | OFF           | ON             | Update read-only variable and restart the server |
| gtid_mode                              | OFF           | ON             | Update read-only variable and restart the server |
| server_id                              | 1             | <unique ID>    | Update read-only variable and restart the server |
| slave_parallel_type                    | DATABASE      | LOGICAL_CLOCK  | Update the server variable                       |
| slave_preserve_commit_order            | OFF           | ON             | Update the server variable                       |
+----------------------------------------+---------------+----------------+--------------------------------------------------+

Some variables need to be changed, but cannot be done dynamically on the server.
NOTE: Please use the dba.configureInstance() command to repair these issues.

{
    "config_errors": [
        {
            "action": "server_update", 
            "current": "COMMIT_ORDER", 
            "option": "binlog_transaction_dependency_tracking", 
            "required": "WRITESET"
        }, 
        {
            "action": "server_update+restart", 
            "current": "OFF", 
            "option": "enforce_gtid_consistency", 
            "required": "ON"
        }, 
        {
            "action": "server_update+restart", 
            "current": "OFF", 
            "option": "gtid_mode", 
            "required": "ON"
        }, 
        {
            "action": "server_update+restart", 
            "current": "1", 
            "option": "server_id", 
            "required": "<unique ID>"
        }, 
        {
            "action": "server_update", 
            "current": "DATABASE", 
            "option": "slave_parallel_type", 
            "required": "LOGICAL_CLOCK"
        }, 
        {
            "action": "server_update", 
            "current": "OFF", 
            "option": "slave_preserve_commit_order", 
            "required": "ON"
        }
    ], 
    "status": "error"
}


表格里面的配置提示告诉用户不满足的配置项,需要我们配置这些信息在my.cnf配置文件中
表格里显示的配置项根据不同版本的MySQL服务器也不尽相同.


my.cnf内容(我这里将my.cnf放到了Linux服务器的/etc文件夹下,然后重启MySQL服务)

[mysqld]
# MySQL集群的唯一标志,每一个MySQL实例都不相同(每个MySQL实例的server_id都不相同,数字自定义)
server_id=300
# 主从复制并行化开启,相比于MySQL5.7来说性能显著提升
binlog_transaction_dependency_tracking=WRITESET
# 开启全局事务ID,保证主从数据库数据一致性
enforce_gtid_consistency=ON
# 开启全局事务ID
gtid_mode=ON
# 从库并行复制(基于锁的并发控制)
slave_parallel_type=LOGICAL_CLOCK
# 保证在从库事务的执行顺序与主库相同
slave_preserve_commit_order=ON


--------------------------------------------------------------------------------------

修改完以上的配置,重启MySQL服务器,然后再MySQL Shell上面执行检查方法

MySQL  192.168.1.100:3306 ssl  JS > dba.checkInstanceConfiguration()

这时候应该就会出现当前MySQL服务节点已经满足了配置集群的要求,这是其中一种解决MySQL实例不满足集群的方式.另一种方式看下面↓↓↓↓↓↓↓↓↓↓↓

```
---

#### 4.4 将不满足配置集群的MySQL实例配置成满足的

* 方式一
```
方式一就是上面介绍的那种方式,手动的在my.cnf中修改配置,让其MySQL实例满足集群要求.
```
* 方式二
```
MySQL官网提供了一个函数,可以直接调用函数修改MySQL实例的默认配置,让其满足集群配置要求.

函数为:dba.configureInstance();

//在MySQL Shell交互模式输入此函数,在执行过程中会让我们输入2次y进行确认,然后就是等待MySQL服务器重启.

MySQL  192.168.1.100:3306 ssl  JS > dba.configureInstance()
Configuring MySQL instance at localhost.localdomain:3306 for use in an InnoDB cluster...

This instance reports its own address as localhost.localdomain:3306
Clients and other cluster members will communicate with it through this address by default. If this is not correct, the report_host MySQL system variable should be changed.

applierWorkerThreads will be set to the default value of 4.

NOTE: Some configuration options need to be fixed:
+----------------------------------------+---------------+----------------+--------------------------------------------------+
| Variable                               | Current Value | Required Value | Note                                             |
+----------------------------------------+---------------+----------------+--------------------------------------------------+
| binlog_transaction_dependency_tracking | COMMIT_ORDER  | WRITESET       | Update the server variable                       |
| enforce_gtid_consistency               | OFF           | ON             | Update read-only variable and restart the server |
| gtid_mode                              | OFF           | ON             | Update read-only variable and restart the server |
| server_id                              | 1             | <unique ID>    | Update read-only variable and restart the server |
| slave_parallel_type                    | DATABASE      | LOGICAL_CLOCK  | Update the server variable                       |
| slave_preserve_commit_order            | OFF           | ON             | Update the server variable                       |
+----------------------------------------+---------------+----------------+--------------------------------------------------+

Some variables need to be changed, but cannot be done dynamically on the server.
Do you want to perform the required configuration changes? [y/n]: y
Do you want to restart the instance after configuring it? [y/n]: y
Configuring instance...
The instance 'localhost.localdomain:3306' was configured to be used in an InnoDB cluster.
Restarting MySQL...
NOTE: MySQL server at localhost.localdomain:3306 was restarted.

待MySQL服务器重启完成之后,在MySQL Shell中检查当前的MySQL服务器是否符合MySQL集群的要求就会发现可以满足了,这种方式比较简单,方便.

```
---

#### 4.5 创建集群

##### 4.5.1 创建集群前准备

```
1. 现在保证有三台安装了MySQL服务器节点
2. 有一台安装了MySQL Shell管理客户端的节点
3. 服务器都关闭了防火墙,禁用了SELinux
```
> 为了方便主机进行连接,我们将这4台主机的IP地址映射成主机名字


主机名称 | 主机地址 | 备注
---|---|---
node1 | 192.168.1.100(IP地址为自己的主机地址) | MySQL
node2 | 192.168.1.101(IP地址为自己的主机地址) | MySQL
node3 | 192.168.1.102(IP地址为自己的主机地址) | MySQL
node4 | 192.168.1.103(IP地址为自己的主机地址) | MySQL Shell、MySQL Router


> 设置hosts(/etc/hosts)

```
192.168.1.103 node1
192.168.1.119 node2
192.168.1.106 node3
192.168.1.117 node4

主机IP地址一定要做名称映射否则可能出现创建集群失败,主机实例添加不到集群中.
```

> 设置好hosts之后使用MySQL Shell进行登陆就不需要在使用IP地址

```

直接使用node1代替IP地址,方便记忆,也是官网推荐方式

MySQL  JS > shell.connect("root@node1:3306")
Creating a session to 'root@node1:3306'
Please provide the password for 'root@node1:3306': ****
Fetching schema names for autocompletion... Press ^C to stop.
Your MySQL connection id is 10
Server version: 8.0.23 MySQL Community Server - GPL
No default schema selected; type \use <schema> to set one.
<ClassicSession:root@node1:3306>


连接成功之后,IP地址也会变成主机名称
MySQL  node1:3306 ssl  JS >
```

##### 4.5.2 集群创建操作

```
1. 使用MySQL Shell管理客户端连接任意一台MySQL服务器,作为集群的主节点,在这个节点上执行创建命令
    
    # 使用dba.createCluster('myCluter')创建一个名字为myCluster的集群,并且声明一个变量,将这个函数的返回值赋给cluster变量
    
    MySQL  node1:3306 ssl  JS > var cluster = dba.createCluster('myCluter')
    A new InnoDB cluster will be created on instance 'node1:3306'.
    
    Validating instance configuration at node1:3306...
    
    This instance reports its own address as node1:3306
    
    Instance configuration is suitable.
    NOTE: Group Replication will communicate with other members using 'node1:33061'. Use the localAddress option to override.
    
    Creating InnoDB cluster 'myCluter' on 'node1:3306'...
    
    Adding Seed Instance...
    Cluster successfully created. Use Cluster.addInstance() to add MySQL instances.
    At least 3 instances are needed for the cluster to be able to withstand up to
    one server failure.
    
    
    显示上面的信息说明创建集群成功,并将此节点创建为主节点(此节点可读写)
    
2. 通过 MySQL Shell查看当前集群的状态(当前集群只有一个MySQL节点)
   
   # cluster这个是上面执行创建集群函数声明的那个变量
   # status() 查看当前集群状态的函数,是MySQL集群提供的简单的监控函数之一
   
    MySQL  node1:3306 ssl  JS > cluster.status()
    {
        "clusterName": "myCluter",    # 集群名称
        "defaultReplicaSet": {
            "name": "default", 
            "primary": "node1:3306",  # 主实例地址
            "ssl": "REQUIRED",       
            "status": "OK_NO_TOLERANCE",    # 当前一个MySQL实例,所以当前集群高可用差,但是运行正常
            "statusText": "Cluster is NOT tolerant to any failures.",  # 当前集群只有一个实例,所以这句话的意思是当前集群不能容忍任何失败
            "topology": {
                "node1:3306": {
                    "address": "node1:3306",  # 当前MySQL实例地址
                    "mode": "R/W",            # 可读写
                    "readReplicas": {}, 
                    "replicationLag": null, 
                    "role": "HA",             # HA(Highly Available)代表这是高可用的设计
                    "status": "ONLINE",       # 运行正常
                    "version": "8.0.23"       # 当前MySQL实例版本
                }
            }, 
            "topologyMode": "Single-Primary"  # 单主模式(后期可通过命令转换成多主)
        }, 
        "groupInformationSourceMember": "node1:3306"  # 当前组的成员(MySQL的数据同步,采用的是组复制)
    }
```
---

#### 4.6 获取集群

```
如果在执行创建集群的命令(var cluster = dba.createCluster('myCluter'))没有声明变量

可以执行此函数(var m = dba.getCluster())重新获取当前集群的JS对象

在使用监控函数(m.status())查看当前集群的状态
```
---

#### 4.7 向集群中添加子节点

```

# cluster: 创建集群声明的JS变量名称
# addInstance(): 向集群中添加MySQL实例的地址

MySQL  node1:3306 ssl  JS > cluster.addInstance("root@node2:3306")

NOTE: The target instance 'node2:3306' has not been pre-provisioned (GTID set is empty). The Shell is unable to decide whether incremental state recovery can correctly provision it.
The safest and most convenient way to provision a new instance is through automatic clone provisioning, which will completely overwrite the state of 'node2:3306' with a physical snapshot from an existing cluster member. To use this method by default, set the 'recoveryMethod' option to 'clone'.

The incremental state recovery may be safely used if you are sure all updates ever executed in the cluster were done with GTIDs enabled, there are no purged transactions and the new instance contains the same GTID set as the cluster or a subset of it. To use this method by default, set the 'recoveryMethod' option to 'incremental'.


Please select a recovery method [C]lone/[I]ncremental recovery/[A]bort (default Clone): C
Validating instance configuration at node2:3306...

This instance reports its own address as node2:3306

Instance configuration is suitable.
NOTE: Group Replication will communicate with other members using 'node2:33061'. Use the localAddress option to override.

A new instance will be added to the InnoDB cluster. Depending on the amount of
data on the cluster this might take from a few seconds to several hours.

Adding instance to the cluster...

NOTE: User 'mysql_innodb_cluster_200'@'%' already existed at instance 'node1:3306'. It will be deleted and created again with a new password.
Monitoring recovery process of the new cluster member. Press ^C to stop monitoring and let it continue in background.
Clone based state recovery is now in progress.

NOTE: A server restart is expected to happen as part of the clone process. If the
server does not support the RESTART command or does not come back after a
while, you may need to manually start it back.

* Waiting for clone to finish...
NOTE: node2:3306 is being cloned from node1:3306
** Stage DROP DATA: Completed
** Clone Transfer  
    FILE COPY  ============================================================    0%  In Progress
    PAGE COPY  ============================================================    0%  Not Started
    REDO COPY  ============================================================    0%  Not Started

NOTE: node2:3306 is shutting down...

* Waiting for server restart... ready
* node2:3306 has restarted, waiting for clone to finish...
** Stage RESTART: Completed
* Clone process has finished: 72.20 MB transferred in about 1 second (~72.20 MB/s)

State recovery already finished for 'node2:3306'

The instance 'node2:3306' was successfully added to the cluster.

出现上面的信息,说明添加成功  这时候使用监控查看状态

MySQL  node1:3306 ssl  JS > cluster.status()
{
    "clusterName": "myCluter", 
    "defaultReplicaSet": {
        "name": "default", 
        "primary": "node1:3306", 
        "ssl": "REQUIRED", 
        "status": "OK_NO_TOLERANCE", 
        "statusText": "Cluster is NOT tolerant to any failures.", 
        "topology": {
            "node1:3306": {
                "address": "node1:3306", 
                "mode": "R/W",           # 读写
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node2:3306": {
                "address": "node2:3306", 
                "mode": "R/O",             # 只读
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }
        }, 
        "topologyMode": "Single-Primary"
    }, 
    "groupInformationSourceMember": "node1:3306"
}


下面可以将第三台也按照第二台的方式添加进集群中,构成了一个高可用的MySQL集群,然后再查看状态


MySQL  node1:3306 ssl  JS > cluster.status()
{
    "clusterName": "myCluter", 
    "defaultReplicaSet": {
        "name": "default", 
        "primary": "node2:3306", 
        "ssl": "REQUIRED", 
        "status": "OK", 
        "statusText": "Cluster is ONLINE and can tolerate up to ONE failure.", 
        "topology": {
            "node1:3306": {
                "address": "node1:3306", 
                "mode": "R/O", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node2:3306": {
                "address": "node2:3306", 
                "mode": "R/W", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node3:3306": {
                "address": "node3:3306", 
                "mode": "R/O", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }
        }, 
        "topologyMode": "Single-Primary"
    }, 
    "groupInformationSourceMember": "node2:3306"
}

```

#### 4.8 如果MySQL集群节点出现故障

##### 4.8.1 全部出现故障
```
如果MySQL服务节点全部挂掉,那么在重新启动MySQL服务器之后,集群不会自动恢复,恢复步骤

1. 将挂掉的MySQL实例全部启动
2. 使用MySQL Shell连接任意一个MySQL实例
3. 使用dba.rebootClusterFromCompleteOutage()函数恢复集群

    MySQL  node2:3306 ssl  JS > dba.rebootClusterFromCompleteOutage()
    Restoring the default cluster from complete outage...
    
    The instance 'node1:3306' was part of the cluster configuration.
    Would you like to rejoin it to the cluster? [y/N]: y    # 想重新加入集群吗(y)
    
    node2:3306 was restored.
    Rejoining 'node1:3306' to the cluster.
    Rejoining instance 'node1:3306' to cluster 'myCluter'...
    The instance 'node1:3306' was successfully rejoined to the cluster.
    
    The cluster was successfully rebooted.
    
    <Cluster:myCluter>

    显示上面的信息就说明恢复成功了这是在交互模式下重新获取集群对象
    
    # 获取集群对象
    MySQL  node2:3306 ssl  JS > var cluster = dba.getCluster();
    # 查看集群状态
    MySQL  node2:3306 ssl  JS > cluster.status();

```
##### 4.8.2 某一台出现故障

* 主节点出现故障
```
如果主节点出现故障,那么从节点会升级成主节点,待挂掉的主节点重新启动之后会自动连接进集群中,在单主中,他就会变成从节点
```
* 从节点出现故障
```
如果从节点出现故障,他就会从集群中退出,但是在重启之后,会自动重新加入到集群中

注意如果节点不自动加入到集群中,官网提供了rejoinInstance("实例地址")函数

MySQL  node2:3306 ssl  JS > cluster.rejoinInstance("实例地址") 

```

##### 4.8.3 解散集群

```
MySQL  node2:3306 ssl  JS > cluster.dissolve()

1. 删除集群中的所有节点
2. 关闭组复制
3. 不会删除数据
```

##### 4.8.5 集群模式切换

```
上面所创建的集群默认为单主模式集群,MySQL官网提供了另外一种设计集群的方式(多主模式),设置多主模式非常简单,官网提供了两种模式切换的API,可以轻松在两种模式中进行切换

# 调用函数进行单主到多主的切换cluster.switchToMultiPrimaryMode()

MySQL  node1:3306 ssl  JS > cluster.switchToMultiPrimaryMode()
Switching cluster 'myCluter' to Multi-Primary mode...

Instance 'node1:3306' remains PRIMARY.
Instance 'node2:3306' was switched from SECONDARY to PRIMARY.
Instance 'node3:3306' was switched from SECONDARY to PRIMARY.

The cluster successfully switched to Multi-Primary mode.


# 切换完成之后查看当前集群的状态

MySQL  node1:3306 ssl  JS > cluster.status()
{
    "clusterName": "myCluter", 
    "defaultReplicaSet": {
        "name": "default", 
        "ssl": "REQUIRED", 
        "status": "OK", 
        "statusText": "Cluster is ONLINE and can tolerate up to ONE failure.", 
        "topology": {
            "node1:3306": {
                "address": "node1:3306", 
                "mode": "R/W", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node2:3306": {
                "address": "node2:3306", 
                "mode": "R/W", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node3:3306": {
                "address": "node3:3306", 
                "mode": "R/W", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }
        }, 
        "topologyMode": "Multi-Primary"   # 多主模式,已经切换完成
    }, 
    "groupInformationSourceMember": "node1:3306"
}


# 当前也可以切换回单主模式cluster.switchToSinglePrimaryMode("root@node1:3306")
# 切换回单主的时候要指定主实例

MySQL  node1:3306 ssl  JS > cluster.switchToSinglePrimaryMode("root@node1:3306")
Switching cluster 'myCluter' to Single-Primary mode...

Instance 'node1:3306' remains PRIMARY.
Instance 'node2:3306' was switched from PRIMARY to SECONDARY.
Instance 'node3:3306' was switched from PRIMARY to SECONDARY.

WARNING: Existing connections that expected a R/W connection must be disconnected, i.e. instances that became SECONDARY.

The cluster successfully switched to Single-Primary mode


# 切换完成在查看当前集群状态

MySQL  node1:3306 ssl  JS > cluster.status()
{
    "clusterName": "myCluter", 
    "defaultReplicaSet": {
        "name": "default", 
        "primary": "node1:3306", 
        "ssl": "REQUIRED", 
        "status": "OK", 
        "statusText": "Cluster is ONLINE and can tolerate up to ONE failure.", 
        "topology": {
            "node1:3306": {
                "address": "node1:3306", 
                "mode": "R/W", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node2:3306": {
                "address": "node2:3306", 
                "mode": "R/O", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }, 
            "node3:3306": {
                "address": "node3:3306", 
                "mode": "R/O", 
                "readReplicas": {}, 
                "replicationLag": null, 
                "role": "HA", 
                "status": "ONLINE", 
                "version": "8.0.23"
            }
        }, 
        "topologyMode": "Single-Primary"   # 切换回单主
    }, 
    "groupInformationSourceMember": "node1:3306"
}
```
---

### 第5节 MySQL Router路由
```
MySQL集群已经配置完成,接下来配置MySQL的路由(轻量级的MySQL的中间件)

MySQL Router路由的主要作用是代理MySQL集群服务,在应用访问时访问MySQL Router而不是直接访问MySQL服务.
```
#### 5.1 下载地址
```
https://dev.mysql.com/get/Downloads/MySQL-Router/mysql-router-8.0.23-linux-glibc2.12-x86_64.tar.xz
```
#### 5.2 安装和环境变量配置

* 安装步骤
```
二进制安装非常简单:

1. 将下载的压缩包复制到/usr/local

2. 解压
   tar -Jxvf mysql-router-8.0.23-linux-glibc2.12-x86_64.tar.xz

3. 重命名  mv mysql-router-8.0.23-linux-glibc2.12-x86_64 mysql-router


安装完成

```
* 环境变量配置
```
环境变量配置

export export PATH=/usr/local/mysql-router/bin:$PATH


环境变量配置完成之后可以执行 mysqlrouter --version 查看一下版本信息

执行 mysqlrouter --help 查看一下帮助文件,里面介绍了MySQL Router的使用方式

```
##### 5.3 配置文件自动生成
```
上面配置完成之后,可以使用命令查看当前MySQL集群被哪一个路由连接.查看命令为 
 MySQL  node1:3306 ssl  JS > cluster.listRouters()
{
    "clusterName": "myCluter", 
    "routers": {}
}
当然我们还没有使用MySQL Router连接集群,所以routers为空

下面就是开始使用Router连接上面创建好的MySQL集群
```

```
#  mysqlrouter: 启动router路由命令
#  --bootstrap: 指定连接哪台机器
#  --user: 使用哪个用于运行路由

[root@node4 mysql-router]# mysqlrouter --bootstrap root@node1:3306 --user=root
Please enter MySQL password for root: 
# Bootstrapping system MySQL Router instance...

- Creating account(s) (only those that are needed, if any)
- Verifying account (using it to run SQL queries that would be run by Router)
- Storing account in keyring
- Adjusting permissions of generated files
- Creating configuration /usr/local/mysql-router/mysqlrouter.conf

# MySQL Router configured for the InnoDB Cluster 'myCluter'

After this MySQL Router has been started with the generated configuration

    $ /etc/init.d/mysqlrouter restart
or
    $ systemctl start mysqlrouter
or
    $ mysqlrouter -c /usr/local/mysql-router/mysqlrouter.conf

the cluster 'myCluter' can be reached by connecting to:

## MySQL Classic protocol (MySQL协议可读写端口号和只读端口号)

- Read/Write Connections: localhost:6446
- Read/Only Connections:  localhost:6447

## MySQL X protocol (MySQL X 协议可读写端口号和只读端口号)

- Read/Write Connections: localhost:64460
- Read/Only Connections:  localhost:64470

出现上面的东西说明MySQL Router配置生成成功.
可以根据上面生成的信息查找配置文件去阅读.
```
##### 5.4 启动MySQL Router
```
命令:
mysqlrouter &


启动理由成功之后可以使用MySQL Shell查看当前集群连接的路由

 MySQL  node2:3306 ssl  JS > cluster.listRouters()
{
    "clusterName": "myCluter", 
    "routers": {
        "node4::system": {
            "hostname": "node4", 
            "lastCheckIn": "2021-03-13 18:53:38", 
            "roPort": 6447, 
            "roXPort": 64470, 
            "rwPort": 6446, 
            "rwXPort": 64460, 
            "version": "8.0.23"
        }
    }
}

目前为止集群出现完成,路由启动成功,以上就是高可用集群实现的方案(可用于生产环境的一种解决方案)
```