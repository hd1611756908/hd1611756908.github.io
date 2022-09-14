---
title: Dubbo快速入门
categories: dubbo
tags: ["dubbo","java","nacos","zookeeper","分布式","RPC"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://ukoko.gitee.io/
B 站: https://space.bilibili.com/514155929/
```

## 第一章 分布式简介

### 第1节 架构演进

<table>
	<tr>
		<td style="font-size:9px;text-align:center;">单一架构</td>
		<td style="font-size:9px;text-align:center;">垂直架构</td>
		<td style="font-size:9px;text-align:center;">分布式(SOA服务治理)架构</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB9e2771b25a233c3077d95c6fe54cc725?method=download&shareKey=8bdd757d2381661df4862d6b72e0dd04">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB28996d72d7e3257185a4c7ffc8a5301f?method=download&shareKey=d2033d213d5c0272a7d3a7f7c7f98667">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBb40b2b48faf3ba8fc2259136a1136ab7?method=download&shareKey=ce6c560fe7c67d8329c7b29e12ea98d6">
		</td>
	</tr>
	<tr>
		<td style="font-size:9px;">网站用户少,流量小,抗压力差(eg: ssm)</td>
		<td style="font-size:9px;">网站用户量进一步增长,流量增多,服务器不能平滑扩容(eg: 多个ssm)</td>
		<td style="font-size:9px;">网站用户和流量随时间稳步升高,需要随时进行服务器扩容(eg: rpc/http)</td>
	</tr>
</table>


<!-- more -->

### 第2节 分布式框架解决的问题

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB32476d0711a21e36c1864e13c0d86602?method=download&shareKey=75c33bfaaadca396a57ef4c0c8268b15">
		</td>
	</tr>
</table>

```
随着互联网架构的越来越复杂,由原来的单一架构 ...到... 流动计算架构 涉及到的服务节点越来越多,这时候就会出现各种各样的问题:
1. 服务的URL地址的管理问题(每一个服务都可能有多个URL地址暴露出去,供别的服务调用)
2. 服务之间的依赖关系问题(服务之间会相互调用)
3. 线上部署的服务越来越多,服务的容量问题不好分配(哪个服务需要增减机器,哪台服务空闲时间过长,哪台服务总是在满载运行等等)

针对以上问题的解决方案当前市场上的技术有很多 Dubbo(阿里巴巴)、Motan(新浪微博)等
```

### 第3节 Dubbo框架

#### 3.1 Dubbo框架介绍

* Dubbo由来

```
Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源 Java RPC框架 

1. 2008年在阿里内部诞生
2. 2011年开源
3. 2012年发布 2.5.3 版本后停止更新  --> 2014年当当发布dubbox,是基于阿里开源的dubbo 2.5.3版本增加rest协议的dubbo版本
4. 2017年阿里重启dubbo项目
5. 2018年进入apache孵化
6. 2019年成为apache顶级项目,同时发布多语言(Erlang,golang,java)dubbo版本
7. 2020年发布dubbo 3.0 部署云原生 当前最高版本dubbo3.0.5 (2022年1月22日)
```


* 使用Dubbo的公司

<img src="https://note.youdao.com/yws/api/personal/file/WEB5886c4a5bffb711751a1be51c7ebbf3a?method=download&shareKey=49de6b7f40c49b45eca2f20901002142">

#### 3.2 Dubbo架构介绍

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBccc29d03047033f587d2296f77817039?method=download&shareKey=aa7b15567303f36091595696303556ff">
		</td>
		<td style="width:50%;font-size:10px;">
			<strong>1.</strong> Provider : 消息提供者
			<br>
			<strong>2.</strong> Consumer : 服务消费者
			<br>
			<strong>3.</strong> Registry : 注册中心
			<br>
			<strong>4.</strong> Monitor  : 统计服务的调用次数和调用时间的监控中心
			<br>
			<strong>5.</strong> Container: 服务运行容器(ioc,log4j,web相关容器(tomcat)等)
		</td>
	</tr>
	<tr>
		<td colspan="2" style="font-size:10px;">
			1. 服务容器负责启动,加载,运行服务提供者.<br>
			2. 服务提供者在启动时,向注册中心注册自己提供的服务.<br>
			3. 服务消费者在启动时,向注册中心订阅自己所需的服务.<br>
			4. 注册中心返回服务提供者地址列表给消费者,如果有变更,注册中心将基于长连接推送变更数据给消费者.<br>
			5. 服务消费者,从提供者地址列表中,基于软负载均衡算法,选一台提供者进行调用,如果调用失败,再选另一台调用.<br>
			6. 服务消费者和提供者,在内存中累计调用次数和调用时间,定时每分钟发送一次统计数据到监控中心.
		</td>
	</tr>
</table>


## 第二章 Dubbo快速入门(Dubbo2.x)

### 第1节 RPC介绍

```
RPC是远程过程调用(Remote Procedure Call)的缩写形式,他不是具体的通信协议,可以把它看成是一种通信模型或者通信设计思想.

```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEB2c69cee72ecabd1f66559c710a160daf?method=download&shareKey=d32abac4d86f19d8ae294ace797e8c48">
		</td>
		<td style="font-size:12px;">
			RPC: 远程过程调用.<br>
			他可以让我们调用远程方法和调用本地方法一样简单.将远程方法调用转换成本地方法调用的形式.<br>
			Dubbo框架就是一个很好RPC的具体实现.内部的调用逻辑由框架来完成,程序员只需要关注使用方式即可.<br>
		</td>
	</tr>
</table>

### 第2节 Dubbo和Spring整合(基于XML)

```
根据第一章第3节 3.2 Dubbo架构图可以看出完成入门案例需要,①服务提供者(必须)、②服务消费者(必须)、③注册中心(必须)、④运行服务提供者的容器(必须)、⑤监控(非必须)

依赖:

<dependency>
	<groupId>org.apache.dubbo</groupId>
	<artifactId>dubbo</artifactId>
	<version>2.7.15</version>
</dependency>

```

* 设置服务

```
RPC远程过程调用,将远程调用转换成本地调用,例如远程有一个add方法,在本地也调用add方法,保证消费者和提供者有相同的方法,所以将服务接口单独抽取出一个项目,让提供者和消费者依赖.

1. 创建普通的Java工程(一般起名字 xxx-api)
2. 定义服务接口

/**
 * @Auther: hushuang
 * @Date: 2022/1/30 18:28
 * @Description: 定义服务接口
 */
public interface DemoService {
    /**
     * 功能方法
     * @param msg
     * @return
     */
    String sayHello(String msg);
}

```

* 服务提供者

```
1. 在pom.xml中添加公共api依赖

2. Java代码

/**
 * @Auther: hushuang
 * @Date: 2022/1/30 18:33
 * @Description: 服务接口的实现
 */
public class DemoServiceImpl implements DemoService {
    @Override
    public String sayHello(String msg) {
		System.out.println("我是服务提供者,我被调用了...");
        return "Hello "+ msg;
    }
}

3. xml配置(provider.xml),将DemoServiceImpl实例服务化

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 设置应用名称 -->
    <dubbo:application name="helloworld-provider"></dubbo:application>
    <!-- 设置注册中心(multicast),采用内置注册中心进行测试 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
    <!-- 将需要暴露出去的服务进行实例化 -->
    <bean class="io.ukoko.service.impl.DemoServiceImpl" id="demoService"></bean>
    <!-- 将实例化的服务使用dubbo进行服务化 -->
    <dubbo:service interface="io.ukoko.service.DemoService" ref="demoService"></dubbo:service>
</beans>

4. 编写Java代码初始化IOC容器,使用IOC容器启动dubbo服务

/**
 * @Auther: hushuang
 * @Date: 2022/1/30 18:40
 * @Description: 服务提供者启动方法
 */
public class ProviderStart {
    public static void main(String[] args) throws IOException {
        ApplicationContext ioc = new ClassPathXmlApplicationContext("provider.xml");
        System.out.println("服务提供者启动成功......");
        System.in.read();//线程停到此位置,按任意键退出
    }
}

```

* 服务消费者(调用服务提供者)


```
1. 在pom.xml中添加公共api依赖

2. xml配置(consumer.xml)生成远程服务代理

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- 设置应用名称 -->
    <dubbo:application name="helloworld-consumer"></dubbo:application>
    <!-- 设置注册中心(multicast),采用内置注册中心进行测试 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
    <!-- 生成远程服务代理,在本地生成远程服务的代理,可以向调用本地方法的方式调用远程法法-->
    <dubbo:reference interface="io.ukoko.service.DemoService" id="demoService"></dubbo:reference>
</beans>

3. 启动客户端,并且调用远程方法

/**
 * @Auther: hushuang
 * @Date: 2022/1/30 19:01
 * @Description: 消费者启动类
 */
public class ConsumerStart {
    public static void main(String[] args) {

        ApplicationContext ioc = new ClassPathXmlApplicationContext("consumer.xml");
        //通IOC容器中获取远程代理对象
        DemoService demoService = (DemoService) ioc.getBean("demoService");
        String msg = demoService.sayHello("Dubbo");
        System.out.println(msg);
    }
}

注意: 在一台电脑上同时启动服务提供者和服务消费者,会报qos server 地址被占用

产生的原因:
	1. Dubbo从2.0.5版本开始支持通过telnet命令来进行服务治理,但是在2.5.8版本之前都是和服务共用20880端口号,2.5.8版本之后使用独立端口号默认为22222
	2. 在同一台服务器上面启服务提供者和服务消费者,会发生端口号冲突问题.
	3. 文档地址:
		3.1 telnet: https://dubbo.apache.org/zh/docs/v2.7/user/references/telnet/
		3.2 qos-server: https://dubbo.apache.org/zh/docs/v2.7/user/references/qos/
		
QoS参数介绍:
参数                 说明              默认值
qosEnable           是否启动QoS        true
qosPort             启动QoS绑定的端口   22222
qosAcceptForeignIp  是否允许远程访问    false

注意: 从2.6.4/2.7.0开始，qosAcceptForeignIp默认配置改为false，如果qosAcceptForeignIp设置为true，有可能带来安全风险

QoS参数可以通过如下方式进行配置:
1. 系统属性(步骤见下图)
   
   在启动main方法之前在IDEA中对main方法的vm启动参数进行配置,配置如下:
   -Ddubbo.application.qos.enable=true
   -Ddubbo.application.qos.port=33333
   -Ddubbo.application.qos.accept.foreign.ip=false
   
2. dubbo.properties
   
   在项目的src/main/resources目录下添加dubbo.properties文件，内容如下:
   
	dubbo.application.qos.enable=true
	dubbo.application.qos.port=33333
	dubbo.application.qos.accept.foreign.ip=false

3. XML方式(建议)

	<dubbo:application name="demo-provider">
		<dubbo:parameter key="qos.enable" value="true"/>
		<dubbo:parameter key="qos.accept.foreign.ip" value="false"/>
		<dubbo:parameter key="qos.port" value="33333"/>
	</dubbo:application>
	
4. Spring-boot自动装配方式
   如果是spring-boot的应用，可以在application.properties或者application.yml上配置
   
   dubbo.application.qosEnable=true
   dubbo.application.qosPort=33333
   dubbo.application.qosAcceptForeignIp=false

其中,上述方式的优先顺序为系统属性 > dubbo.properties > XML/Spring-boot自动装配方式

```


<table>
	<tr>
		<td colspan="2" style="font-size:12px;text-align:center;">系统属性配置方式</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB15ada07d82a65bccdddbc71b10a27f9e?method=download&shareKey=0a9fb89c36cd1a04718d3acd2b4bd887">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB2c848c3ab838f5a7003d0334b1a5d09b?method=download&shareKey=2b31f3e3191852d9f8c491d3e63bffd3">
		</td>
	</tr>
</table>


### 第3节 注册中心(zookeeper)

```
Dubbo框架官网建议使用zookeeper注册中心,multicast注册中心只能用于测试环境,不能用于生产环境

zookeeper的安装和使用请查看【zookeeper简单入门课程】

注意事项: 
	1. 如果使用dubbo2.7.x版本 那么zookeeper的依赖直接使用【zookeeper简单入门课程】提供的即可
	2. 如果使用dubbo3.x版本 那么zookeeper的依赖不能使用【zookeeper简单入门课程】提供的依赖

如果使用 dubbo3.x 使用如下依赖:

<!-- zookeeper客户端依赖 start -->
<dependency>
	<groupId>org.apache.curator</groupId>
	<artifactId>curator-x-discovery</artifactId>
	<version>4.2.0</version>
</dependency>
<!-- zookeeper客户端依赖 end -->

zookeeper依赖来源于dubbo源码,本地zookeeper服务使用3.4.9版本

```

### 第4节 Dubbo管理控制台

```
Dubbo运维管理指南中简介了Dubbo Admin管理控制台,官网地址: https://dubbo.apache.org/zh/docs/v2.7/admin/ops/

Dubbo 管理控制台的功能介绍:
	1. 服务查询
	2. 服务治理
	3. 服务测试

Dubbo 管理控制台的部署方式:
	1. maven方式部署
	2. 前后端分离部署

Dubbo管理控制台源码地址: 
	1. GitHub地址: git clone https://github.com/apache/dubbo-admin.git (官网)
	2. GitEE地址: git clone https://gitee.com/whathz/dubbo-admin.git (个人临时)

Dubbo管理控制台maven部署方式介绍:
	1. 下载源码
	2. 在 dubbo-admin-server/src/main/resources/application.properties中修改注册中心地址
	3. 构建(编译和打包) mvn clean package -Dmaven.test.skip=true
	4. 运行 
		4.1 cd dubbo-admin-distribution/target
		4.2 java -jar dubbo-admin-0.1.jar
	5. 访问: http://localhost:8080
	6. 默认用户名/密码  root/root

注意: 启动控制台之前需要先启动zookeeper服务器
```

<table>
	<tr>
		<td colspan="2" style="font-size:12px;text-align:center;">控制台成功标志</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBf06679e4bea91ec405f3c2ef257af3f0?method=download&shareKey=3388cb3f50c6c15ede930ed331b847b8">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBd66fc6f77c0df895c9f351e916f93376?method=download&shareKey=1dee4926e81f9188b1666100d2595344">
		</td>
	</tr>
</table>

### 第5节 Dubbo应用集群部署

```
如果是开发阶段,可以直接使用main方法运行即可,但是如果正式环境,需要打包部署

本章测试使用Spring的IOC容器进行运行.容器介绍地址:
https://dubbo.apache.org/zh/docs/v2.7/user/examples/service-container/#spring-container
```

* 项目打包

```
将项目打成一个可执行的jar包进行进行.

第一步: pom.xml 设置

插件官网地址介绍: https://maven.apache.org/plugins/maven-shade-plugin/

<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-shade-plugin</artifactId>
	<version>3.2.4</version>
	<executions>
		<execution>
			<!--
				运行maven周期命令
				将shade插件的shade周期转成maven的package周期
			-->
			<phase>package</phase>
			<goals>
				<goal>shade</goal>
			</goals>
			<configuration>
				<transformers>
					<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
						<!-- 设置可执行jar包的入门类,使用Dubbo官网自带的启动类 -->
						<mainClass>org.apache.dubbo.container.Main</mainClass>
					</transformer>
				</transformers>
				<!--
					shadedArtifactAttached: true/false
						1. true : 表示打包会生成两个
						2. false: 也会生成两个包
						3. 设置为true: 会生成以下面的标签shadedClassifierName设置的名字结尾的文件
				-->
				<shadedArtifactAttached>true</shadedArtifactAttached>
				<shadedClassifierName>executable</shadedClassifierName>
			</configuration>
		</execution>
	</executions>
</plugin>

第二步: 修改resources下的spring配置文件位置

使用Spring的IOC容器启动项目,那么Spring默认加载resources目录下的META-INF\spring 目录下的文件 so...

1. resources目录下创建META-INF目录和spring目录
2. 将spring配置文件移动到此目录中

第三步: 修改resources\META-INF\spring下的spring配置文件内容

在生产环境很少在同一个机器节点上部署多个dubbo服务,也就是一台服务器上不会部署多个dubbo服务;但是在是开发测试阶段,会在同一台机器上部署多个dubbo服务节点,
dubbo服务设置的端口号是20880,在同一个服务器上启动多个dubbo服务会出现端口号冲突.这是怎么解决:

解决方案:
	在spring配置文件中修改此标签 <dubbo:protocol name="dubbo" port="20880"></dubbo:protocol>
	将20880端口设置成-1,当设置成-1之后dubbo服务会根据当前硬件资源占用情况自行设置端口号,默认从20880开始递增.
	
	例如: <dubbo:protocol name="dubbo" port="-1"></dubbo:protocol>
	
第四步: 打包 mvn clean package -Dmaven.test.skip=true

```

* 项目运行

```
使用java -jar xxx.jar执行dubbo服务,运行成功在Dubbo-Admin管理控制台查看

使用java -jar xxx.jar 启动同一个jar文件多次,构成集群服务

```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBe207591bcdfc72a0c02283a0f404c644?method=download&shareKey=b88e35397aa971b3cd2c745e0ae9ad8e">
		</td>
	</tr>
</table>

* 脚本运行(Linux)

```
启动jar的脚本


#!/bin/bash

# 进入当前脚本所在目录
cd `dirname $0`
# 获取当前目录
BIN_DIR=`pwd`
# 退出上一层
cd ..
# 进入servers目录
cd servers
# 获取部署目录
DEPLOY_DIR=`pwd`
# 输出应用部署位置
echo "应用所在位置---> $DEPLOY_DIR"
# 日志目录
cd ../logs
LOG_DIR=`pwd`
# 输出部署文件列表
for app in $(ls $DEPLOY_DIR)
do
	echo "部署的应用---> $app"
	# 判断应用是否运行
	PIDS=`ps -ef | grep java | grep -v grep | grep "$app" | awk '{print $2}'`
	if [ -n "$PIDS" ]; then
        echo "服务正在运行中...."
		echo "PIDS: $PIDS"
		exit 1
    fi
	
	# 运行应用
	echo -e "Starting the $app ..."
	# 执行运行脚本,使用&进行后台运行
	nohup java -jar $DEPLOY_DIR/$app > $LOG_DIR/stdout.log 2>&1 &
	# 查询是否运行成功
	PIDS=`ps -ef | grep java | grep -v grep | grep "$app" | awk '{print $2}'`
	echo "PID: $PIDS"
	echo "运行成功..."
done

注意: 官网提供了启动脚本 地址为:
https://gitee.com/apache/dubbo/tree/3.0/dubbo-container/dubbo-container-api/src/main/resources/META-INF/assembly/bin
```

### 第6节 Dubbo和SpringBoot整合(使用注解方式: 需要 2.6.3 及以上版本支持)
```
注意: 非SpringBoot也可以使用Dubbo注解,本节使用SpringBoot的自动装配,官网地址:

1. GITHUB地址: https://github.com/apache/dubbo-spring-boot-project
2. GITEE地址 : https://gitee.com/mirrors/Dubbo-Spring-Boot-Project

当前测试的SpringBoot版本为2.5.9 Dubbo(dubbo-spring-boot-starter)的版本为3.0.5

注解配置地址(老版本,新版本的修改会在代码中体现): https://dubbo.apache.org/zh/docs/v2.7/user/configuration/annotation/
```

#### 6.1 服务提供方

* 依赖

```

1. xxx-api公共依赖

2. 其他依赖

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
	<version>2.5.9</version>
</dependency>
<dependency>
	<groupId>org.apache.dubbo</groupId>
	<artifactId>dubbo-spring-boot-starter</artifactId>
	<version>3.0.5</version>
</dependency>
<dependency>
	<groupId>org.apache.curator</groupId>
	<artifactId>curator-x-discovery</artifactId>
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
	<exclusions>
		<exclusion>
			<artifactId>slf4j-log4j12</artifactId>
			<groupId>org.slf4j</groupId>
		</exclusion>
		<exclusion>
			<artifactId>slf4j-api</artifactId>
			<groupId>org.slf4j</groupId>
		</exclusion>
	</exclusions>
</dependency>

```

* 本地服务实现

```
package io.ukoko.dubbo.provider.service;
import io.ukoko.dubbo.api.DemoService;
import org.apache.dubbo.config.annotation.DubboService;

@DubboService(version = "1.0.0")
public class DemoServiceImpl implements DemoService {
    @Override
    public String sayHello(String name) {
        System.out.println("name:"+name);
        return "Hello "+name;
    }
}

```

* 配置

```
# 设置dubbo应用名称
dubbo.application.name=springboot-provider
# 配置注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 服务协议和端口号
dubbo.protocol.name=dubbo
dubbo.protocol.port=-1
# 配置dubbo服务化的包,扫描apache dubbo提供的@Service[2.7.0以前]注解,2.7.0之后注解修改成了@DubboService
dubbo.scan.base-packages=io.ukoko.dubbo.provider.service
```

* 启动

```
@SpringBootApplication
public class DubboProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(DubboProviderApplication.class, args);
    }
}

打成jar包启动多个进行测试(使用Dubbo-Admin)
```

#### 6.2 服务消费方

* 依赖

```
同上,与提供者相同
```

* 配置

```
# 设置dubbo应用名称
dubbo.application.name=springboot-consumer
# 配置注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

* 测试代码

```
import io.ukoko.dubbo.api.DemoService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.boot.ApplicationRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class DubboConsumerApplication {

    /*
        @Reference在Dubbo2.7.0版本之前使用
        @DubboReference 在Dubbo2.7.0版本之后使用
     */
    @DubboReference(version = "1.0.0")
    private DemoService demoService;

    public static void main(String[] args) {
        SpringApplication.run(DubboConsumerApplication.class, args);
    }

    @Bean
    public ApplicationRunner testService(){
        return args->System.out.println(demoService.sayHello("Dubbo..."));
    }
}
```

## 第三章 Dubbo最佳实践

```
Dubbo官网提供了官网的推荐用法: https://dubbo.apache.org/zh/docsv2.7/user/recommend/

Dubbo配置:
	1. 介绍 Dubbo各个功能、策略的成熟度: https://dubbo.apache.org/zh/docsv2.7/user/maturity/
	2. 详细配置介绍: https://dubbo.apache.org/zh/docsv2.7/user/configuration/
	3. 用法示例 : https://dubbo.apache.org/zh/docsv2.7/user/examples/
	4. 参考手册: https://dubbo.apache.org/zh/docsv2.7/user/references/
	...
```

## 第四章 Nacos作为注册中心(不建议)

```
Nacos服务器的安装和配置,请参考【nacos简单入门】

使用nacos作为注册中心替换原有的zookeeper操作起来非常简单:
1. 启动本地的nacos服务
2. 在项目中注释掉zookeeper的客户端依赖,并且添加nacos客户端依赖

<!-- springboot 2.0.3 ~ 2.6.3 ,如果springboot版本过高可能nacos客户端不支持,因为nacos普通版为社区共建版本,可能与SpringBoot的更新不同步-->
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>nacos-discovery-spring-boot-starter</artifactId>
	<version>0.2.10</version>
</dependency>

3. 修改配置文件application.properties中注册中心的地址

# 配置注册中心
dubbo.registry.address=nacos://127.0.0.1:8848
# 配置元数据中心,如果不配置Dubbo-Admin控制台会不显示服务中的方法列表
dubbo.metadata-report.address=nacos://127.0.0.1:8848

4. 配置好之后启动服务提供者

5. 查看nacos控制台的服务列表(也可以查看Dubbo-Admin控制台),Dubbo-Admin配置:只需要将一些注释取消即可

# nacos注册中心
admin.registry.address=nacos://127.0.0.1:8848?group=DEFAULT_GROUP&namespace=public
admin.config-center=nacos://127.0.0.1:8848?group=dubbo
admin.metadata-report.address=nacos://127.0.0.1:8848?group=dubbo

# nacos默认使用名称空间
admin.registry.namespace=public
admin.config-center.namespace=public
admin.metadata-report.namespace=public


注意: 
	5.1 Dubbo-Admin控制台如果注册中心切换成nacos需要重新配置,否则不展示服务列表或者方法列表
	5.2 官网建设比较烂,没有提供具体配置方案,在github上面issues(#659)没有及时解决  issues地址: https://github.com/apache/dubbo-admin/issues/659
	5.3 官网在issues(#694)解决了部分问题  issues地址: https://github.com/apache/dubbo-admin/issues/694
	5.3 但是在dubbo-admin的项目中dubbo-admin\dubbo-admin-server\src\main\resources\application.properties配置文件中,可以通过注释查看nacos配置方式.
	5.4 个人不建议在中小型公司在内部项目上使用社区共建的技术(如果有商业版可以使用商业版),除非自己公司内部本身就有强大的技术团队.
```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB83d5bf513ef88024e8e4a47e54f65f0f?method=download&shareKey=720398d80a30d1e094d48f8e5bd5f403">
		</td>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBdef4244da04cda0bd0d7e9efcf423a72?method=download&shareKey=e67bc15915a062856f1c01999dc04a39">
		</td>
	</tr>
</table>



