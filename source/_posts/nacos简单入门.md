---
title: nacos简单入门
categories: nacos
tags: ["nacos","java","CAP","分布式配置中心","注册中心","CP","AP"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://ukoko.gitee.io/
B 站: https://space.bilibili.com/514155929/
```

## 第一章 nacos简介

```
Nacos主要帮助我们发现、注册、配置和管理微服务

1. 注册中心功能(Dubbo或者SpringCloud介绍)
2. 配置中心功能
3. 服务管理功能(Dubbo或者SpringCloud介绍)


官网地址: https://nacos.io/zh-cn/index.html
```

<!-- more -->

## 第二章 nacos环境

```
1. nacos 依赖 Java 环境来运行
2. 如果源码构建nacos需要安装maven
3. 64 bit OS (windows/Linux/Unix/Mac)  推荐选用 Linux/Unix/Mac
4. 64 bit JDK 1.8+
5. Maven 3.2.x+
```

## 第三章 nacos下载和安装

### 第1节 nacos下载

```
1. 源码下载
	git clone https://gitee.com/mirrors/Nacos.git
	cd Nacos/
	mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U
	ls -al distribution/target/
	
	编译完成之后生成 nacos-server-2.0.3.tar.gz 和 nacos-server-2.0.3.zip安装包
2. 编译之后版本下载(网络慢可以去国内镜像源码编译)
	https://github.com/alibaba/nacos/releases
```

### 第2节 nacos安装

```
测试安装采用单点安装

编译好之后再/Nacos/distribution/target/nacos-server-2.0.3/nacos/bin路径下使用命令操作
cd /Nacos/distribution/target/nacos-server-2.0.3/nacos/bin

1. Linux/Unix/Mac/ubuntu系统

	1.1 Linux/Unix/Mac
		启动命令: sh startup.sh -m standalone
		停止命令: sh shutdown.sh
	
	1.2 ubuntu
		命令: bash startup.sh -m standalone

2. Windows系统
	启动命令: startup.cmd -m standalone
	停止命令: shutdown.cmd
	
	windows杀死进程的方式
	1. 查询进程(通过端口号)  netstat -ano | findstr 8080    最后一列为进程号
	2. 杀死进程(通过进程号)  taskkill /F /PID 11380
```
### 第3节 nacos控制台访问

```
从日志中可以查看访问控制台的地址,如果是windows访问地址直接在控制台显示,如果是Linux等其他需要自己去日志目录查看日志.

访问密码默认为: nacos/nacos
```

<img src="https://note.youdao.com/yws/api/personal/file/WEB5a1e26e2e5343bcfafbf4c9708cbe231?method=download&shareKey=edded08ce4c08472b3c1c91354ac05ac">


## 第四章 nacos配置管理(操作台)

### 第1节 名称空间(命名空间)管理

* 命名空间介绍

```
命名空间可以帮助用户更好的管理测试、预发、生产等多环境服务和配置,让每个环境的同一个配置（如数据库数据源）定义不同的值
```

* 命名空间创建和修改

<img src="https://note.youdao.com/yws/api/personal/file/WEB53f1b4d2fb6dcf12f003df4a34b24387?method=download&shareKey=5962e9a30bf7c587268bcb5ce31392c2">

### 第2节 配置管理

<img src="https://note.youdao.com/yws/api/personal/file/WEB3e270ee20fa29cbc64a133174207bde7?method=download&shareKey=eeaaf01994f569d28489ab5db185f0be">

<img src="https://note.youdao.com/yws/api/personal/file/WEBace35f2e4afc8c6ee3210f390367693d?method=download&shareKey=33a952b198717bc1b5d5f106d79c50e3">

<table>
	<tr>
		<th>参数名</th>
		<th>参数类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<th>dataId</th>
		<td>string</td>
		<td style="font-size:12px;">配置 ID，采用类似 package.class（如com.taobao.tc.refund.log.level）的命名规则保证全局唯一性，class 部分建议是配置的业务含义。全部字符小写。只允许英文字符和 4 种特殊字符（"."、":"、"-"、"_"），不超过 256 字节</td>
	</tr>
	<tr>
		<th>group</th>
		<td>string</td>
		<td style="font-size:12px;">配置分组，建议填写产品名:模块名（Nacos:Test）保证唯一性，只允许英文字符和4种特殊字符（"."、":"、"-"、"_"），不超过128字节</td>
	</tr>
</table>

<img src="https://note.youdao.com/yws/api/personal/file/WEBfb7eef054739c76bf092428395a9cbe7?method=download&shareKey=10c8d8bf520a070ae77229ae41f3ce47">

```
如果想了解更详细的配置信息请访问: https://nacos.io/zh-cn/docs/console-guide.html
```

## 第五章 nacos配置管理

### 第1节 Java的SDK使用

* maven依赖

```
<!-- nacos客户端依赖 -->
<dependency>
	<groupId>com.alibaba.nacos</groupId>
	<artifactId>nacos-client</artifactId>
	<version>2.0.3</version>
</dependency>
<!-- 依赖guava -->
<dependency>
	<groupId>com.google.guava</groupId>
	<artifactId>guava</artifactId>
	<version>31.0.1-jre</version>
</dependency>
<!-- 依赖slf4j -->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.21</version>
</dependency>
```

* 数据操作

```
/**
 * 获取nacos配置
 */
@Test
public void test01() throws NacosException {
	Properties properties = new Properties();
	/*
		设置地址
	 */
	properties.put(PropertyKeyConst.SERVER_ADDR,"127.0.0.1:8848");
	/*
		设置名称空间
		命名空间的值是命名空间ID,不能是命名空间的名字
		如果不设置命名空间,那么默认读取自带的public空间
	 */
	properties.put(PropertyKeyConst.NAMESPACE,"26df001f-7eaa-49eb-95fe-8a28f6acdb7e");
	/*
		获取配置服务实例
	 */
	ConfigService configService = NacosFactory.createConfigService(properties);
	/*
		获取配置实例
		第一个参数(dataId): 命名空间下的自定义的 dataId 值
		第二个参数(group):  命名空间下的自定义的 group 值
		第三个参数(timeoutMs): 读取数据的超时时间
	 */
	String config = configService.getConfig("tmall", "DEFAULT_GROUP", 3000);
	System.out.println(config);
}


/*
 * 监听数据变化
 */
@Test
public void test02() throws NacosException {
	
	Properties properties = new Properties();
	/*
				   设置地址
	 */
	properties.put(PropertyKeyConst.SERVER_ADDR,"127.0.0.1:8849");
	/*
			设置名称空间
			命名空间的值是命名空间ID,不能是命名空间的名字
			如果不设置命名空间,那么默认读取自带的public空间
	 */
	properties.put(PropertyKeyConst.NAMESPACE,"26df001f-7eaa-49eb-95fe-8a28f6acdb7e");
	/*
		获取配置服务实例
	 */
	ConfigService configService = NacosFactory.createConfigService(properties);
	/*
			获取配置实例
			第一个参数(dataId): 命名空间下的自定义的 dataId 值
			第二个参数(group):  命名空间下的自定义的 group 值
			第三个参数(timeoutMs): 读取数据的超时时间
	 */
	String config = configService.getConfig("tmall", "DEFAULT_GROUP", 3000);
	System.out.println(config);
	//给当前配置(configService)添加监听器
	configService.addListener("tmall", "DEFAULT_GROUP", new Listener() {
		
		public void receiveConfigInfo(String configInfo) {
			System.out.println("我是监听器我被触发了...配置信息为:"+configInfo);
		}
		
		public Executor getExecutor() {
			return null;
		}
	});
	/*
	 * 防止程序执行完退出,保持进程不退出.
	 * 在正式项目中项目进程不需要这段代码守护,可以删除
	 */
	while (true) {
		try {
			Thread.sleep(1000);
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
			System.out.println("监听中...当前时间: "+sdf.format(new Date(System.currentTimeMillis())));
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

请多案例请去官网查看: https://nacos.io/zh-cn/docs/sdk.html
```


### 第2节 Nacos在Spring中的使用(传统ssm)

* 依赖

```
<dependency>
  <groupId>com.alibaba.nacos</groupId>
  <artifactId>nacos-spring-context</artifactId>
  <version>1.1.1</version>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.0.1</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>3.2.18.RELEASE</version>
</dependency>

注意: 
	1. nacos-spring-context使用1.1.1版本的依赖时内嵌spring框架版本为3.2.18.RELEASE
	2. nacos和spring框架整合spring版本不是重点,所以springmvc依然使用3.2.18.RELEASE版本做测试
	3. 官网采用web的方式进行测试,所以案例中引入了springmvc
```

* 案例步骤

* * 第1步: 创建JavaWeb项目,添加依赖(上面)
* * 第2步: 配置springmvc

```
2.1 web.xml中配置

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"
		 xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
		 metadata-complete="true" version="3.0">
	<!--配置springmvc的核心控制器-->
	<servlet>
		<servlet-name>dispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>dispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>

2.2 配置springmvc核心配置文件(dispatcherServlet-servlet.xml)

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
	<!--配置扫描包 -->
	<context:component-scan base-package="io.ukoko"></context:component-scan>
</beans>
```

* * 第3步: 配置nacos(采用Java类配置方式)

```
import com.alibaba.nacos.api.annotation.NacosProperties;
import com.alibaba.nacos.spring.context.annotation.config.EnableNacosConfig;
import com.alibaba.nacos.spring.context.annotation.config.NacosPropertySource;
import org.springframework.context.annotation.Configuration;
@Configuration  //设置当前类为配置类
@EnableNacosConfig(globalProperties = @NacosProperties(serverAddr = "127.0.0.1:8848")) //开启nacos配置,制定其nacos服务器地址
@NacosPropertySource(dataId = "tmail",groupId = "DEFAULT_GROUP",autoRefreshed = true) //nacos配置中心基础配置
public class NacosConfiguration {

}


注意: nacos官网使用的是此种配置方式(Java类配置),但是一般在ssm项目中我们常使用xml配置,如果使用xml配置,那么不需要当前配置类,只需要在dispatcherServlet-servlet.xml中添加以下配置即可

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:nacos="http://nacos.io/schema/nacos"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://nacos.io/schema/nacos http://nacos.io/schema/nacos.xsd">
    <!--配置扫描包 -->
    <context:component-scan base-package="io.ukoko"></context:component-scan>

    <!--
        开启nacos注解驱动
    -->
    <nacos:annotation-driven></nacos:annotation-driven>

    <!--
        配置nacos服务器
    -->
    <nacos:global-properties server-addr="127.0.0.1:8848"></nacos:global-properties>

    <!--
        nacos服务器数据配置
    -->
    <nacos:property-source data-id="tmail" group-id="DEFAULT_GROUP" auto-refreshed="true"></nacos:property-source>
    
</beans>

```


<table>
	<tr>
		<td style="font-size:12px;text-align:center;">
			配置文件的样例
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBe78f6ec0b9a4770ba14d9d43ac795e8a?method=download&shareKey=18a67c81545543575a9c74294284dd9e">
		</td>
	</tr>
</table>


* * 第4步: 编写测试代码 

```
@Controller
public class NacosController {
	/* 
		@NacosValue注解: 获取nacos配置中心配置文件.
		${db.name}表达式: nacos配置为properties db.name为自定义的key
		autoRefreshed属性: 自动刷新,如果配置文件有改动,会自动刷新
	 */
	@NacosValue(value = "${db.name}",autoRefreshed = true)
	private String userName;

	@NacosValue(value = "${db.password}",autoRefreshed = true)
	private String password;

	@NacosValue(value = "${db.url}",autoRefreshed = true)
	private String url;

	@ResponseBody
	@RequestMapping(value = "/test02",method = RequestMethod.GET)
	public String test02(){
		return userName+"="+password+"="+url;
	}
}
```

### 第3节 SpringBoot使用

* 依赖

```
<!-- spring web依赖 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- nacos-springboot依赖 -->
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>nacos-config-spring-boot-starter</artifactId>
	<!-- 注意:0.2.x版本nacos对应springboot2.x版本,但是不能高于2.4.x版本,我这里使用springboot的2.3.9.RELEASE版本 -->
	<version>0.2.10</version>
</dependency>

/*  注意: 如果项目中使用的SpringBoot的版本过高没有相对应的nacos依赖那么请使用传统API通过自定义配置方式进行功能实现  */

```

* 案例

```

1. 配置文件 application.properties

# 配置文件中只需要指定nacos服务器地址即可
nacos.config.server-addr=127.0.0.1:8848

2. Java代码

@Controller
@SpringBootApplication
/*配置名称空间*/
@EnableNacosConfig(globalProperties = @NacosProperties(namespace = "a79dd942-05e1-49d8-b85c-a87f94bf4544"))
/*配置data-id*/
@NacosPropertySource(dataId = "tmail",autoRefreshed = true)
public class NacosSpringbootApplication {

    public static void main(String[] args) {
        SpringApplication.run(NacosSpringbootApplication.class, args);
    }

    /**
     * 指向nacos配置中心的属性
     * ${db.user}: 表达式获取配置中心配置db.user自定义的key
     * autoRefreshed: 是否开启自动刷新,不开启修改配置,此属性不生效
     */
    @NacosValue(value = "${db.user}",autoRefreshed = true)
    public String userName;

    /**
     * 获取配置文件的测试方式
     */
    @ResponseBody
    @RequestMapping(value = "/test",method = RequestMethod.GET)
    public String test(){
        return userName;
    }
}

```


## 第六章 nacos的高可用

```
集群搭建注意事项:  官网介绍了在Linux/Unix下集群安装,集群必须节点至少3个以上,其他请查看官网 https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html

搭建步骤: (本文在虚拟机里模拟,Linux操作系统为Centos)

第1步: 在/home/hs文件夹创建nacos-cluster文件夹   mkdir nacos-cluster
第2步: 在nacos-cluster文件夹文件夹下创建3个文件夹 mkdir nacos1 nacos2 nacos3
第3步: 上传文件nacos-server-2.0.4.tar.gz(开篇编译nacos源码生成[不要在windows下编译源码])分别到nacos1、nacos2、nacos3文件夹中
第4步: 将nacos1、nacos2、nacos3文件夹中的nacos-server-2.0.4.tar.gz解压  tar -zxvf nacos-server-2.0.4.tar.gz
第5步: 配置文件修改
	5.1 解压完成之后生成一个nacos文件夹,由于3个节点都在同一台服务器上,所以需要修改每一个节点的默认端口号(原端口号为8848)
	5.2 分别修改nacos1、nacos2、nacos3中的nacos默认端口号,默认端口号修改位置 /home/hs/nacos-cluster/nacos*/nacos/conf/application.properties,端口号修改为7777 8888 9999
	5.3 修改集群配置
		5.3.1 分别将/home/hs/nacos-cluster/nacos1/nacos/conf/cluster.conf.example 这个配置文件名字修改为 cluster.conf
		5.3.2 分别配置三个节点中的cluster.conf文件,在三个配置文件中添加以下配置(ip:port),ip地址不要用本机回环地址
		192.168.80.128:7777
		192.168.80.128:8888
		192.168.80.128:9999
第6步: 启动集群(nacos依赖JAVA_HOME,如果虚拟机内存过小会造成启动失败,至少大于4G)
	6.1 启动命令: 我这里使用内置数据源 命令为: sh startup.sh -p embedded
	6.2 成功标志: 在所有服务器日志中打印 Nacos started successfully in cluster mode. use embedded storage
	6.3 测试: 任意登录一个图形界面,设置配置,会被同步到其他服务器节点
```

<table>
	<tr>
		<td style="font-size:12px;text-align:center;">集群启动成功</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB1f4c656640ff28840946db767ab04f79?method=download&shareKey=53e0013030a5c55f24463888ed6aed02">
		</td>
	</tr>
</table>

## 第七章 nacos集群操作

### 第1节 Java SDK版本

```

/**
 * 配置中心: 集群操作
 * 第五章#第1节 案例类似
 */
@Test
public void test01() throws NacosException {
	Properties properties = new Properties();
	/*
		设置地址
		如果为集群,需要用逗号分割即可
		集群如果有3个点,如果集群down掉2个那么集群不会在提供服务,如果只down掉1个,那么集群正常提供服务
	 */
	properties.put(PropertyKeyConst.SERVER_ADDR,"192.168.80.128:7777,192.168.80.128:8888,192.168.80.128:9999");
	/*
		获取配置服务实例
	 */
	ConfigService configService = NacosFactory.createConfigService(properties);
	/*
		获取配置实例
		第一个参数(dataId): 命名空间下的自定义的 dataId 值
		第二个参数(group):  命名空间下的自定义的 group 值
		第三个参数(timeoutMs): 读取数据的超时时间
	 */
	String config = configService.getConfig("tmail", "DEFAULT_GROUP", 3000);
	System.out.println(config);
}

```

### 第2节 SpringBoot版本

```
使用第五章#第3节案例 只需要在applicaton.properties中将nacos服务器地址修改为 nacos.config.server-addr=192.168.80.128:7777,192.168.80.128:8888.192.168.80.128:9999

注意事项:
	1. 集群正常提供服务的策略和第七章第1节介绍的相同
	2. 不同的地方是当集群中有节点down掉之后修改配置中心的配置,在springboot应用中获取不到最新的配置
	3. 集群如果没有节点down掉,在修改配置中心文件的时候,不影响本地获取最新修改的配置信息
```

## 第八章 nacos一致性协议

### 第1节 CAP定理简介

```
CAP定理,又被称作布鲁尔定理,指在一个分布式系统中的一致性(Consistency),可用性(Availability),分区容错性(Partition tolerance)英文缩写.

在分布式系统中系统要么保证CP要么保证AP,不可能同时满足CAP
```

### 第2节 nacos一致性协议介绍

```
一致性协议介绍来源于nacos官网顶部菜单栏【NACOS架构与原理】(需要登录阿里云)
```

#### 2.1 nacos作为注册中心

```
nacos作为注册中心:保证的是AP
```

#### 2.2 nacos作为配置中心

```
nacos作为配置中心:保证的是CP
```
