---
title: zookeeper简单入门
categories: zookeeper
tags: ["zookeeper","java","zk","分布式配置中心","注册中心","config-toolkit"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://ukoko.gitee.io/
B 站: https://space.bilibili.com/514155929/
```


## 第一章 zookeeper简介

### 第1节 zookeeper的由来

```
1. zookeeper最早起源于雅虎研究院的一个研究小组
2. 在雅虎内部很多大型系统基本都需要依赖一个类似的系统来进行分布式协调,并且这个系统还有单点问题
3. 雅虎的开发人员就试图开发一个通用的无单点问题的分布式协调框架,以便让开发人员将精力集中在处理业务逻辑上,这是最早zookeeper的原型
4. 后来捐献给了apache成为了apache的顶级项目
```

<!-- more -->

### 第2节 zookeeper名字的由来

```
雅虎研究院很多项目都以动物命名,而zookeeper做为各个项目之间的协调,就起了一个动物管理员的名字(zookeeper),简称zk
```

### 第3节 zk应用场景

```
1. 注册中心(Dubbo框架介绍)
2. 配置中心
3. 分布式锁(本文不做讲解)
...

```

## 第二章 zk的下载

```
这里以3.4.9版本为例,下面是下载地址,下载zookeeper-3.4.9.tar.gz压缩包(在Windows/Linux/Mac下通用)

https://archive.apache.org/dist/zookeeper/zookeeper-3.4.9/
```

## 第三章 zk的单点安装

```
1. 平台支持
	1.1 Windows 服务端和客户端开发和生产环境都支持
	1.2 Linux   服务端和客户端开发和生产环境都支持
	
2. 软件环境
	2.1 客户端和服务器端都是用Java语言编写,需要本机安装JDK
	2.2 zookeeper3.4.9版本需要JDK版本大于等于1.6
	
3. 单点安装步骤
	3.1 下载zookeeper安装包
	3.2 解压zookeeper
	3.3 在conf文件夹下找到zoo_sample.cfg将其重命名为zoo.cfg
	3.4 修改配置
		# zookeeper服务器心跳检测
		tickTime=2000
		# zookeeper存放数据的目录,zookeepr服务器可以保存数据
		dataDir=/tmp/zookeeper
		# 服务器启动占用的端口号,用于客户端连接使用
		clientPort=2181
4. 启动和停止
	4.1 启动
		- Windows: bin目录下直接双击 zkServer.cmd
		- Linux  : 使用命令行工具,在bin目录下 zkServer.sh start
	4.2 停止
		- Windows: ctrl+c 或者 使用命令杀进程
		- Linux  : 使用命令行工具,在bin目录下 zkServer.sh stop
```

## 第四章 zk客户端使用

### 第1节 客户端连接

```
连接命令: zkCli.sh -server 127.0.0.1:2181
```

### 第2节 zk节点概述

#### 2.1 zk节点概念

```
zk可以看成是一个小型的数据存储系统,他的数据存储结构和Unix文件系统类型类似(树形结构).
```

<table>
	<tr>
		<td style="text-align:center;font-size:10px;">zk节点图例</td>
		<td style="text-align:center;font-size:10px;">Unix/Linux文件系统图例</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB75337e57182c1aac3c41660a7fc52996?method=download&shareKey=6e6030a13ede57921894329316c7663b">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBb37b63f7f7b4417b376e3d8e82c5e095?method=download&shareKey=95e242a2684d3a65cf708611e2a0acd7">
		</td>
	</tr>
</table>

```
1. zk的数据存储在树形结构的节点中(例如根节点/、app1节点、app2节点以及子节点等)
2. 每个节点都可以存储数据,还可以创建新的子节点
3. 根节点默认存在,其他节点可以手动操作(创建和销毁)
4. 可以使用zk命令创建不同类型的节点,不同类型的节点拥有不同的特性(持久化/非持久化/顺序等)
```

#### 2.2 zk节点类型

```
zk节点类型分为4种:
1. 持久节点(PERSISTENT) : 
	1.1 zk客户端连接zk服务器端会话(session)结束后数据不会丢失
	1.2 持久节点在其下方可以创建子节点
	
2. 临时节点(EPHEMERAL) :
	2.1 zk客户端连接zk服务器端会话(session)结束后数据会丢失
	2.2 临时节点在其下方不可以创建子节点
	
3. 持久顺序节点(PERSISTENT_SEQUENTIAL) :
	3.1 zk客户端连接zk服务器端会话(session)结束后数据不会丢失
	3.2 持久节点在其下方可以创建子节点
	3.3 持久节点在创建同名节点时不能创建成功,但是持久顺序节点可以
	3.4 创建持久顺序节点时,zk会在名称后自动添加一个序号,序号是一个单调递增的计数器,由父节点维护
	
4. 临时顺序节点(EPHEMERAL_SEQUENTIAL) :
	4.1 zk客户端连接zk服务器端会话(session)结束后数据会丢失
	4.2 临时节点在其下方不可以创建子节点
	4.3 临时节点在创建同名节点时不能创建成功,但是临时节点顺序节点可以
	4.3 创建持久顺序节点时,zk会在名称后自动添加一个序号,序号是一个单调递增的计数器,由父节点维护
```

#### 2.3 zk节点操作

* zk常见命令

```
0. help命令 -- 查看当前环境下都可以使用哪些命令

1. ls命令 -- 查看某一个路径下的目录列表    例如: ls /  查看根目录下的目录列表

2. ls2命令 -- 查看某个路径下目录列表(相比于ls命令信息更详细)  例如: ls2 /   查看根目录下的目录列表

3. create命令 -- 创建节点并赋值  例如: create /ukoko val1   创建了一个ukoko节点,并在此节点存储了val1字符串
	3.1 create的格式为  create [-s] [-e] path data acl
	3.2 [-s] [-e] 中括号的参数可有可无 -s表示创建顺序节点  -e表示创建临时节点 不加默认创建持久节点
	3.3 path 指定要创建节点的路径
	3.4 data 要在此节点存储的数据
	3.5 acl 访问权限相关,默认是 world,全世界都能访问
	
4. get命令 -- 获取节点数据和状态信息 例如: get /ukoko   查看当前节点下的数据以及节点固有属性信息
	4.1 get格式  get path [watch]
	4.2 path 要查询的节点路径
	4.3 [watch] 中括号的参数可有可无, watch对节点进行事件监听  例如: get /ukoko watch   查看这个节点,并且监听这个节点数据变化,测试这个节点时需要开启至少两个客户端
	
5. set命令 -- 修改节点存储的数据  例如: set  /ukoko 100 给ukoko节点设置数据
	5.1 set格式 set path data [version]
	5.2 [version] 中括号的参数可有可无  version版本号
	5.3 path 要设置数据的节点路径
	5.4 data 需要存储的数据

6. delete命令 -- 删除某节点     例如: delete /ukoko
	6.1 delete格式 delete path [version]
	6.2 path 要删除的节点路径
	6.3 [version] 中括号的参数可有可无  version版本号(同set命令)

7. stat命令 -- 查看节点状态信息   例如: stat /ukoko
	7.1 stat格式 stat path [watch]
	7.2 [watch]中括号的参数可有可无

8. rmr 命令 -- 强制删除节点以及下子节点  例如 rmr /app
```

* zk节点属性详情

<table>
	<thead>
		<tr>
			<th>序号</th>
			<th>属性</th>
			<th>数据类型</th>
			<th>描述</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>cZxid</td>
			<td>long</td>
			<td>该节点被创建时的事物id</td>
		</tr>
		<tr>
			<td>2</td>
			<td>ctime</td>
			<td>long</td>
			<td>该节点被创建时的创建时间</td>
		</tr>
		<tr>
			<td>3</td>
			<td>mZxid</td>
			<td>long</td>
			<td>该节点最后被修改时的事务id</td>
		</tr>
		<tr>
			<td>4</td>
			<td>mtime</td>
			<td>long</td>
			<td>该节点最后修改时间</td>
		</tr>
		<tr>
			<td>5</td>
			<td>pZxid</td>
			<td>long</td>
			<td>
				该节点子节点列表最后一次修改的事物id(如果修改子节点里面的数据内容那么事物id不会变化,添加或者删除子节点时此id会变化)
			</td>
		</tr>
		<tr>
			<td>6</td>
			<td>cversion</td>
			<td>long</td>
			<td>子节点版本号(子节点每次修改数据版本号+1)</td>
		</tr>
		<tr>
			<td>7</td>
			<td>dataVersion</td>
			<td>long</td>
			<td>数据版本号(数据每次修改该版本号+1)</td>
		</tr>
		<tr>
			<td>8</td>
			<td>aclVersion</td>
			<td>long</td>
			<td>权限版本号(权限每次修改该版本号+1)</td>
		</tr>
		<tr>
			<td>9</td>
			<td>ephemeralOwner</td>
			<td>long</td>
			<td>创建该临时节点的会话的sessionId,如果不是临时节点那么为0</td>
		</tr>
		<tr>
			<td>10</td>
			<td>dataLength</td>
			<td>int</td>
			<td>该节点的数据长度</td>
		</tr>
		<tr>
			<td>11</td>
			<td>numChildren</td>
			<td>int</td>
			<td>该节点子节点的数量</td>
		</tr>
	</tbody>
</table>

* zk常见命令使用

```
注意: 所有命令设计到的路径操作必须是绝对路径,不可以是相对路径
```

* * 创建节点

```
1. 创建持久化节点:
	1.1 命令 create /ukoko val0  创建ukoko节点,并且存储数据val0
	1.2 命令 create /ukoko val1  创建ukoko节点,并且存储数据val1,失败报 Node already exists: /ukoko
	
2. 创建顺序持久化节点:
	2.1 命令 create -s /ukoko  val1 创建ukoko节点,并且存储数据val1
	2.2 命令 create -s /ukoko  val2 创建ukoko节点,并且存储数据val2,成功,因为顺序节点会在节点名称后自动添加序号,使用 ls命令查看
	
3. 创建临时节点
	3.1 命令 create -e /ukoko1  val0 创建ukoko1临时节点,并且存储数据val0
	3.2 使用quit退出命令退出当前会话,会在控制台打印出当前会话id和ephemeralOwner属性对比
	3.3 重新使用客户端命令连接zk服务器,使用ls命令查看,临时节点消失
	
4. 创建顺序临时节点
	4.1 命令 create -s -e /ukoko2 val0 创建ukoko2顺序临时节点,并且存储数据val0
	4.2 命令 create -s -e /ukoko2 val1 创建ukoko2顺序临时节点,并且存储数据val1,成功,因为顺序节点会在节点名称后自动添加序号,使用 ls命令查看
	4.3 使用quit退出命令退出当前会话,会在控制台打印出当前会话id和ephemeralOwner属性对比
	4.3 重新使用客户端命令连接zk服务器,使用ls命令查看,临时节点消失
```

* * 给节点设置数据

```
1. 设置普通数据(如果set设置值时不加版本,那么数据版本会随着每次set递增,可以在每次set成功之后查看dataVersion属性)
	1.1 命令 set /ukoko 100
	1.2 查看数据 get /ukoko
2. 设置带版本数据(如果带版本set,那么必须保证版本号与dataVersion版本相同,保证修改的是当前版本数据)
	2.1 命令 set /ukoko 100 1   如果设置数据的版本,与dataVersion不相同会报version No is not valid(版本号无效)
	2.2 查看数据 get /ukoko
```

* * 获取节点

```
1. 获取普通数据: get /ukoko
2. 获取数据并且添加监听: get /ukoko watch

查看节点数据,并使用watch监听之后,如果对数据节点进行set操作,那么监听器会自动回调  NodeDataChanged path:/ukoko
```

* * 查看节点状态

```
1. 查看节点状态: stat /ukoko
2. 查看节点状态并对节点进行监听: stat /ukoko watch

查看节点状态,并使用watch监听之后,如果对数据节点进行set操作,那么监听器会自动回调  NodeDataChanged path:/ukoko
```

* * 删除节点

```
1. 删除节点
	1.1 命令 delete /ukoko 删除节点
2. 删除带版本的节点
	2.1 命令 delete /ukoko 1 如果后面带有版本那么要和dataVersion版本号相同
```

### 第3节 使用Java客户端操作znode

```
zk的Java客户端常见的有两种:
1. 官网提供的zk客户端           功能简单,使用不便,比较底层
2. Apache提供的curator客户端    功能丰富,使用方便
```

#### 3.1 zk客户端依赖

```
/*  zk官网提供  */
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.9</version>
</dependency>

/* 
	apache提供
	curator官网: https://curator.apache.org/
	如果zookeeper服务版本使用3.4.x 对应的curator版本4.2.x
*/

<dependency>
	<groupId>org.apache.curator</groupId>
	<artifactId>curator-recipes</artifactId>
	<version>4.2.0</version>
	<exclusions>
		<exclusion>
			<groupId>org.apache.zookeeper</groupId>
			<artifactId>zookeeper</artifactId>
		</exclusion>
	</exclusions>
</dependency>
<dependency>
	<groupId>org.apache.zookeeper</groupId>
	<artifactId>zookeeper</artifactId>
	<version>3.4.9</version>
</dependency>
```

#### 3.2 节点数据操作

##### 3.2.1 zookeeper操作

```
@Test
public void test01() throws IOException, KeeperException, InterruptedException {
	//服务器地址: 格式：ip:port ,如果是集群 ip1:port1,ip2:port2,ip3:port3
	String connectString="127.0.0.1:2181";
	//会话超时时间
	int sessionTimeout=3000;

	//创建zk连接
	ZooKeeper zk = new ZooKeeper(connectString, sessionTimeout, new Watcher() {
		/**
		 * Watcher: 监控所有被触发的事件
		 * 触发执行process方法
		 */
		@Override
		public void process(WatchedEvent event) {
			System.out.println("事件触发: "+event);
		}
	});
	//创建一个持久化节点
	//zk.create("/app1","val".getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
	//创建一个持久化子节点在app1节点下
	//zk.create("/app1/app1_1","val1".getBytes(),ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);

	//获取/app1节点数据
	byte[] d1 = zk.getData("/app1", false, null);
	System.out.println("d1= "+new String(d1));
	//获取/app1/app1_1节点数据
	byte[] c1 = zk.getData("/app1/app1_1", false, null);
	System.out.println("c1= "+new String(c1));
	//获取子节点目录列表
	List<String> cList = zk.getChildren("/app1", false);
	System.out.println("cList= "+cList);

	//修改目录节点数据(如果版本为-1,那么版本自动增加)
	//zk.setData("/app1","valllll".getBytes(),-1);
	//修改子目录节点数据
	//zk.setData("/app1/app1_1","valLLLLL".getBytes(),-1);

	/*
		删除节点(如果存在子节点,不能删除父节点)
		版本如果为-1,那么删除任意版本
	 */
	zk.delete("/app1/app1_1",-1);
	zk.delete("/app1",-1);
}
```

##### 3.2.2 curator操作


* curator基本操作

```
@Test
public void test02() throws Exception {
	//服务器地址: 格式：ip:port ,如果是集群 ip1:port1,ip2:port2,ip3:port3
	String connectString="127.0.0.1:2181";
	/*
		重试机制
		1. 等待重试时间
		2. 重试次数
	 */
	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,3);
	//创建zk连接(方式一)
	//CuratorFramework client = CuratorFrameworkFactory.newClient(connectString, retryPolicy);

	//创建zk连接(方式二 Fluent风格[连缀风格的称呼])
	CuratorFramework client = CuratorFrameworkFactory.builder()
			.connectString(connectString)
			.sessionTimeoutMs(5000) //会话超时时间
			.connectionTimeoutMs(2000) //连接超时时间
			.retryPolicy(retryPolicy)
			.build();


	//启动客户端
	client.start();

	/*数据节点操作*/

	/*
		1. 创建持久节点(不设置值)
	 */
	//client.create().forPath("/app1"); //默认创建持久节点

	/*
		2. 创建持久节点(设置值)
	 */
	//client.create().forPath("/app2","我是值".getBytes());

	/*
		3. 创建顺序持久节点(使用withMode函数设置)
	 */
	//client.create().withMode(CreateMode.PERSISTENT_SEQUENTIAL).forPath("/app3","我是值3".getBytes("utf-8"));

	/*
		4. 创建持久节点(如果父节点不存在,递归创建)
	 */
	//父节点不存在创建子节点报异常(org.apache.zookeeper.KeeperException$NoNodeException: KeeperErrorCode = NoNode for /app4/app4_1)
	//client.create().forPath("/app4/app4_1","我是值4".getBytes());
	//父节点不存在创建子节点可以递归创建(creatingParentContainersIfNeeded方法可以递归创建)
	//client.create().creatingParentContainersIfNeeded().withMode(CreateMode.PERSISTENT).forPath("/app5/app5_2","我是值6".getBytes("utf-8"));


	/*
		5. 删除数据节点
	 */
	//client.delete().forPath("/app");

	/*
		6. 删除节点(递归删除其所有子节点)
	 */
	//client.delete().deletingChildrenIfNeeded().forPath("/app4");

	/*
		7. 获取节点数据
	 */
	byte[] s2 = client.getData().forPath("/app2");
	System.out.println("app2节点数据: "+new String(s2));

	/*
		8. 设置节点数据
	 */
	//更新节点数据
	//client.setData().forPath("/app2","我是值2".getBytes("utf-8"));
	//更新节点数据带版本(版本设置-1位默认值,默认递增,如果随便设置值回报异常[BadVersionException])
	//client.setData().withVersion(-1).forPath("/app2","我是修改值2".getBytes("utf-8"));

	/*
		9. 检查节点是否存在
	 */
	Stat stat = client.checkExists().forPath("/app2");
	System.out.println("当前节点详情: "+stat);

	/*
		10. 获取某个节点的所有子节点路径
	 */
	List<String> list = client.getChildren().forPath("/app5");
	list.forEach(System.out::println);

}
```

* curator高级操作

```
@Test
public void test03() throws Exception {
	//服务器地址: 格式：ip:port ,如果是集群 ip1:port1,ip2:port2,ip3:port3
	String connectString="127.0.0.1:2181";
	/*
		重试机制
		1. 等待重试时间
		2. 重试次数
	*/
	RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,3);
	//创建zk连接(方式二 Fluent风格[连缀风格的称呼])
	CuratorFramework client = CuratorFrameworkFactory.builder()
			.connectString(connectString)
			.sessionTimeoutMs(5000) //会话超时时间
			.connectionTimeoutMs(2000) //连接超时时间
			.retryPolicy(retryPolicy)
			.build();

	//开启客户端
	client.start();

	//给某一个节点下的所有子节点添加监听(修改节点),多次操作节点只触发一次监听
	client.getChildren().usingWatcher(new CuratorWatcher() {
		@Override
		public void process(WatchedEvent event) throws Exception {
			System.out.println("事件: "+event.toString());
		}
	}).forPath("/");

	//创建/app1节点(事件触发)
	//client.create().creatingParentContainersIfNeeded().forPath("/app1");

	//设置值(事件不触发)
	//client.setData().forPath("/app1","app1".getBytes());

	//删除节点(事件触发)
	//client.delete().forPath("/app1");
}
```

## 第五章 zookeeper的高可用


* zk集群介绍

```
单点zk服务在正式环境很容易造成单点故障,导致服务器不可用,为了解决这个问题,需要将zk服务部署成集群,可以解决单点问题.

zk集群的设计:
	1. CAP定理之CP设计
		1.1 CPA定理概念(Consistency[一致性]、Availability[可用性]、Partition tolerance[分区容错性])
		1.2 在分布式系统中不可能保证CAP全部实现,一般提供CP或者AP服务
		1.3 zk是一个典型的CP设计
	2. 集群过半可存活(过半提供服务,低于一半主机不对外提供服务)
		2.1 集群部署节点数一般为奇数个,因为过半可存活原理偶数可能浪费机器(3个节点和4个节点是一样的)
		2.2 防止脑裂(一个zk集群[5个节点由于网络原因分成了3个和2个单独成为了各自的集群],这就是脑裂)
		2.3 过半可存活策略可以很好的解决出现脑裂造成数据不一致的问题
```

* 集群配置

```
我们以3个实例节点作为例子部署,正常每一个节点分别在不同的机器上面,由于硬件有限,将3个节点部署到一台机器上进行测试


部署方式和但实力节点差不多,就是需要修改多份配置文件
```

* * zk1实例

```
1. 解压zk压缩包
2. 在解压文件中创建data(在data文件中创建myid文件,并且存一个服务器编号)和log文件夹 和bin目录平级即可
3. 修改conf中的zoo_sample.cfg配置文件,重命名为zoo.cfg
4. 修改配置文件内容,内容如下:

# 心跳检测(zk服务器和zk客户端之间每隔一个tickTime检查一次是否还连着[毫秒])
tickTime=2000
# zk和zk之间(leader和follower)初始化连接时能忍受几个tickTime的个数,超过个数没有连接,剔除
initLimit=10
# zk和zk(leader和follower)之间发送消息请求和应答的时间长度多少个tickTime数量
syncLimit=5
# 数据存储位置
dataDir=../data
# 事务日志存储位置
dataLogDir=../log
# 端口号
clientPort=2181
# 集群配置 server.A=B.C.D  解释: server集群配置属性(固定值) 1:和myid中存的值相同用于表示当前zk的服务编号 B:服务器主机IP C: zk服务器之间通信端口 D:zk选举端口
server.1=127.0.0.1:2555:3555
server.2=127.0.0.1:2556:3556
server.3=127.0.0.1:2557:3557

```

* * zk2实例

```
1. 解压zk压缩包
2. 在解压文件中创建data(在data文件中创建myid文件,并且存一个服务器编号)和log文件夹 和bin目录平级即可
3. 修改conf中的zoo_sample.cfg配置文件,重命名为zoo.cfg
4. 修改配置文件内容,内容如下:

# 心跳检测(zk服务器和zk客户端之间每隔一个tickTime检查一次是否还连着[毫秒])
tickTime=2000
# zk和zk之间(leader和follower)初始化连接时能忍受几个tickTime的个数,超过个数没有连接,剔除
initLimit=10
# zk和zk(leader和follower)之间发送消息请求和应答的时间长度多少个tickTime数量
syncLimit=5
# 数据存储位置
dataDir=../data
# 事务日志存储位置
dataLogDir=../log
# 端口号
clientPort=2182
# 集群配置 server.A=B.C.D  解释: server集群配置属性(固定值) 2:和myid中存的值相同用于表示当前zk的服务编号 B:服务器主机IP C: zk服务器之间通信端口 D:zk选举端口
server.1=127.0.0.1:2555:3555
server.2=127.0.0.1:2556:3556
server.3=127.0.0.1:2557:3557

```

* * zk3实例

```
1. 解压zk压缩包
2. 在解压文件中创建data(在data文件中创建myid文件,并且存一个服务器编号)和log文件夹 和bin目录平级即可
3. 修改conf中的zoo_sample.cfg配置文件,重命名为zoo.cfg
4. 修改配置文件内容,内容如下:

# 心跳检测(zk服务器和zk客户端之间每隔一个tickTime检查一次是否还连着[毫秒])
tickTime=2000
# zk和zk之间(leader和follower)初始化连接时能忍受几个tickTime的个数,超过个数没有连接,剔除
initLimit=10
# zk和zk(leader和follower)之间发送消息请求和应答的时间长度多少个tickTime数量
syncLimit=5
# 数据存储位置
dataDir=../data
# 事务日志存储位置
dataLogDir=../log
# 端口号
clientPort=2183
# 集群配置 server.A=B.C.D  解释: server集群配置属性(固定值) 3:和myid中存的值相同用于表示当前zk的服务编号 B:服务器主机IP C: zk服务器之间通信端口 D:zk选举端口
server.1=127.0.0.1:2555:3555
server.2=127.0.0.1:2556:3556
server.3=127.0.0.1:2557:3557

```

```
将各个节点启动,然后使用客户端连接其中的某一个进行测试.
```

## 第六章 配置中心

### 第1节 在Java项目中时时获取zk数据

```
举例: 以京东宙斯开放平台举例,平台地址为 https://jos.jd.com/

场景说明: 
1. 我有一个项目,要从京东宙斯平台提供的接口获取数据,京东宙斯地址假设为  192.168.1.100
2. 恰逢双十一我们平台可能非常频繁的调用京东宙斯的接口,京东宙斯平台也会根据双十一调整机房的容量,调整外部应用的访问地址调整为容量更大的 192.168.1.200
3. 由于切换地址正常情况下需要我们停机器,修改项目本地配置,然后在重新启动,这样的缺点就是需要停服务.有没有一种方式可以不停服务直接进行动态的地址切换呢?
4. zk的配置中心可以帮助我们解决这个问题
```

#### 1.1 案例实现一

```
1. 创建JavaWeb项目,添加zk客户端依赖
2. 将数据保存到zk节点中
3. 在项目中使用zk客户端获取zk服务器节点数据

以JavaWeb项目是ssm项目为例
```

* Spring配置文件

```
<!--
	实例化zk客户端
	CuratorFrameworkFactory静态工厂类
	设置工厂方法 factory-method为newClient,IOC容器实例化这个工厂类时返回的对象是newClient的返回值对象
	init-method：设置初始化方法为start,工厂类实例化完成后启动客户端
-->
<bean id="client" class="org.apache.curator.framework.CuratorFrameworkFactory" factory-method="newClient" init-method="start">
	<!--zk集群地址,多个使用逗号分隔-->
	<constructor-arg index="0" value="127.0.0.1:2181"></constructor-arg>
	<!--会话超时时间-->
	<constructor-arg index="1" value="5000"></constructor-arg>
	<!--连接超时时间-->
	<constructor-arg index="2" value="3000"></constructor-arg>
	<!--重试机制-->
	<constructor-arg index="3" ref="retry"></constructor-arg>
</bean>
<!--
	重试机制
-->
<bean id="retry" class="org.apache.curator.retry.ExponentialBackoffRetry">
	<constructor-arg index="0" value="3000"></constructor-arg>
	<constructor-arg index="1" value="3"></constructor-arg>
</bean>
```

* 控制器中调用

```
//IOC容器注入
@Autowired
private CuratorFramework client;

@RequestMapping(value = "/hello2")
public String hello2() throws Exception {
	//获取节点数据
	byte[] bytes = client.getData().forPath("/app");
	String s = new String(bytes, "utf-8");
	System.out.println("s="+s);
	return "ok";
}
```

#### 1.2 案例实现二

```
案例一缺点就是每次都需要zk客户端去数据节点查询数据,性能较差(网络延时)
案例二将zk服务器数据节点数据缓存到内存中,如果数据节点发生变化,使用zk监听器监听并修改缓存

案例二建立在案例一 配置文件基础之上

Curator监听器是对zookeeper基本watch的包装,可以实现循环注册和监听主要涉及到三个监听器:

1. 监听某一个节点的子节点变化  --  PathChildrenCache
2. 监听特定的某一个节点        -- NodeCache
3. 监听某一个节点的所有的后代节点  -- TreeCache
```

* 编写监听器

* * 监听器一(监听某一个节点的子节点变化)

```
import org.apache.curator.framework.CuratorFramework;
import org.apache.curator.framework.recipes.cache.PathChildrenCache;
import org.apache.curator.framework.recipes.cache.PathChildrenCacheListener;
/**
 * 监听某一个点的所有子节点
 */
public class ChildrenCacheListenerTool1 {
    /**
     * 构造器
     * @param client
     * @param path
     * @throws Exception
     */
    public ChildrenCacheListenerTool1(CuratorFramework client, String path) throws Exception {
        /*
            监听某一个点下的子节点
            第一个参数: zk客户端
            第二个参数: 节点路径
            第三个参数: 如果为true缓存数据节点内容
         */
        PathChildrenCache cache = new PathChildrenCache(client, path, true);
        //开启PathChildrenCache
        cache.start();
        /*
            设置监听,创建PathChildrenCacheListener监听对象
            第一个参数: zk客户端
            第二个参数: 事件对象
         */
        PathChildrenCacheListener listener = (x,event)->{
            System.out.println("节点名称: "+event.getData().getPath());
            System.out.println("事件名称:"+event.getType()+"<<--->>节点数据: "+(event.getData()!=null ? new String(event.getData().getData(),"utf-8"):"数据为空"));
            //节点数据发生变化监听执行,将数据写到缓存中,比如写到redis
        };
        //添加监听
        cache.getListenable().addListener(listener);
    }
}
```

* * 监听器二(监听特定的某一个节点变化)

```

```

* * 监听器三(监听某一个节点的所有的后代节点)

```
import org.apache.curator.framework.CuratorFramework;
import org.apache.curator.framework.recipes.cache.NodeCache;
import org.apache.curator.framework.recipes.cache.NodeCacheListener;
/**
 * 监听某一个特定节点
 */
public class ChildrenCacheListenerTool2 {
    /**
     * 构造器
     * @param client
     * @param path
     * @throws Exception
     */
    public ChildrenCacheListenerTool2(CuratorFramework client, String path) throws Exception {
        /*
            监听某一个特定节点
            第一个参数: zk客户端
            第二个参数: 节点路径
         */
        NodeCache cache = new NodeCache(client, path);
        //开启NodeCache
        cache.start();
        //设置监听,创建NodeCacheListener监听对象
        NodeCacheListener listener = ()->{
            System.out.println("app节点名称: "+cache.getPath());
            System.out.println("app节点数据: "+(cache.getCurrentData()!=null ? new String(cache.getCurrentData().getData(),"utf-8"):"节点被删除空"));
            //节点数据发生变化监听执行,将数据写到缓存中,比如写到redis
        };
        //添加监听
        cache.getListenable().addListener(listener);
    }
}
```
* * 将三个监听配置到xml配置文件中

```
<!--
	注册监听某一个点的所有子节点
-->
<bean class="io.ukoko.listeners.ChildrenCacheListenerTool1">
	<constructor-arg index="0" ref="client"></constructor-arg>
	<constructor-arg index="1" value="/"></constructor-arg>
</bean>

<!--
	注册监听某一个特定节点
-->
<bean class="io.ukoko.listeners.ChildrenCacheListenerTool2">
	<constructor-arg index="0" ref="client"></constructor-arg>
	<constructor-arg index="1" value="/app"></constructor-arg>
</bean>
<!--
	注册监听某个节点下的所有节点,包含后代节点
-->
<bean class="io.ukoko.listeners.ChildrenCacheListenerTool3">
	<constructor-arg index="0" ref="client"></constructor-arg>
	<constructor-arg index="1" value="/"></constructor-arg>
</bean>
```
* * 使用zkCli.sh或者zkCli.cmd 测试

```
使用zk的常见命令在客户端进行操作,查看监听是否会被执行...
```

### 第2节 Config-Toolkit可视化配置中心

#### 2.1 Config-Toolkit简介

```
介绍: Config-Toolkit是一款由当当网开源的分布式配置管理工具,通过web界面维护属性配置和配置的导入导出功能

特性:
	1. 集中管理集群配置
	2. 实现配置热更新
	3. 多配置源支持，内置支持zookeeper、本地文件、http协议
	4. Spring集成
	5. web界面管理配置
	...

环境:
	1. JAVA 7+
	2. TOMCAT 7+ for ConfigWeb
```

#### 2.2 Config-Toolkit下载地址

```
1. GitEE地址  : https://gitee.com/mirrors/Config-Toolkit
2. GitHub地址 : https://github.com/dangdangdotcom/config-toolkit
```

#### 2.3 Config-Toolkit安装

```
1. 下载源码  git clone https://gitee.com/mirrors/Config-Toolkit.git
2. 编译源码  mvn clean package -Dmaven.test.skip=true 
3. 运行: 
	3.1 进入项目目录  cd Config-Toolkit\config-face\target
	3.2 执行运行命令  java -jar config-face.jar --zk=localhost:2181
```

#### 2.4 Config-Toolkit使用

* 访问

```
config-face项目运行起来之后 通过地址访问: http://localhost:8080  需要用户名和密码

1. 用户名/密码创建
	1.1 启动zookeeper服务器,使用zkCli.sh客户端连接上服务器
	1.2 创建/projectx节点,并赋初始化值   命令  create /projectx 1
	1.3 创建子节点create /projectx/modulex a9993e364706816aba3e25717850c26c9cd0d89d 并赋初始化值,这个初始化值是一个使用sha1加密之后的密文(本文密文为官网提供对应明文abc)
	1.4 用户名和密码创建完成(密码密文对应明文为abc,用户名为/projectx/modulex这个根节点目录)
2. 访问http://localhost:8080这个地址使用 用户名:/projectx/modulex 和 密码: abc 登录即可进入页面

下面是第一次登录成功的页面

```

<table>
	<tr>
		<td style="font-size:12px;text-align:center;">登陆页面</td>
		<td style="font-size:12px;text-align:center;">登陆成功初始化页面</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB6bc43e0148ec9cb8171795ec662d6f44?method=download&shareKey=b69393e2b42212ad7e876511f2e0b904">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBd0726e08fc0eeab8c700edaf0f2657c7?method=download&shareKey=cdadceeb16e68767bba605cfad2b22a3">
		</td>
	</tr>
</table>

* 页面配置文件操作

```
1. 创建版本
2. 创建群组
3. 选中群组并且在群组中设置key-value键值对的配置信息
```

<img src="https://note.youdao.com/yws/api/personal/file/WEB134023e11301692196643204f95658d4?method=download&shareKey=d9204cdd6ab11be04933de5b3a947e6e">

<img src="https://note.youdao.com/yws/api/personal/file/WEB43d73524c47cce02d098f6a071c1af56?method=download&shareKey=ef272735f6ffff8dd266dc5580887424">

<img src="https://note.youdao.com/yws/api/personal/file/WEBa5e0a13dd7e6198425f511665812ac22?method=download&shareKey=c3195d9de6287bceca65af05a07cf497">

#### 2.5 Config-Tools API使用

* 添加依赖

```
<dependency>
  <groupId>com.dangdang</groupId>
  <artifactId>config-toolkit</artifactId>
  <version>3.3.2-RELEASE</version>
</dependency>
```

* 通过API获取配置信息

```
//创建ZookeeperConfigProfile加载zk的/projectx/modulex节点的1.0版本的配置
ZookeeperConfigProfile configProfile = new ZookeeperConfigProfile("127.0.0.1:2181", "/projectx/modulex", "1.0");
//创建GeneralConfigGroup获取群组名称为group1的群组
GeneralConfigGroup group = new ZookeeperConfigGroup(configProfile,"group1");
//从群组中获取数据(通过key获取)
String jdIp = group.get("jd_ip");

支持热更新,在config-face操作页面修改配置,不需要重启应用直接获取
```

#### 2.6 Config-Tools集成Spring

* spring配置文件的头约束

```
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:config="https://crnlmchina.github.io/config"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
             https://crnlmchina.github.io/config https://crnlmchina.github.io/config/config.xsd">
```

* 基础配置

```
<!--
	配置Config-Toolkit
-->
<!-- 1. 基础配置 -->
<config:profile connect-str="127.0.0.1:2181" root-node="/projectx/modulex" version="1.0"></config:profile>
<!-- 
	2. 群组配置 
		node属性: 操作页面设置的群组名称
		id属性: 自定义,下面获取群组数据是需要
-->
<config:group node="group1" id="gProp"></config:group>

<!-- 
	3. 自定义一个Java类,将gProp中的数据set到自定义的类对象中 
	
	public class ZkConfig {
		private String jdIp1;
		private String jdIp2;
	}
	从群组或获取数据采用SpEL表达式方式#{goup['key']} group为<config:group> 标签中的id属性值中括号里的key是操作界面自定义的key值
-->
<bean id="zkConfig" class="io.ukoko.ZkConfig">
	<property name="jdIp1" value="#{gProp['jd_ip']}"></property>
	<property name="jdIp2" value="#{gProp['jd_ip2']}"></property>
</bean>

注意:xml配置默认不支持热更新
```

* Java代码

```
//注入
@Autowired
private ZkConfig zkConfig;

@RequestMapping(value = "/hello4")
public String hello4(){
	System.out.println("zk配置信息: "+zkConfig);
	return "ok";
}
```