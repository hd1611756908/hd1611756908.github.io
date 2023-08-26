---
title: 03-Java语言概述
categories: javase
tags: ["后端","java","javase","jdk8","jdk0","环境变量","JAVA_HOME"]
---

```xml
作者 : 夜泊1990
邮箱 : hd1611756908@163.com
企鹅 : 1611756908
鹅群 : 948233848
博客 : https://hd1611756908.github.io/
破站 : https://space.bilibili.com/514155929/
```

## 第一章 Java语言发展史
```xml
硬件的发展 -- 廉价的单片式微控制器 -- 用于智能化家居

1991年成立Green小组(詹姆斯·高斯林任组长),专攻计算机在家电领域的嵌入式应用
1992年夏Oak开发成功(根据C++语言改造),Oak开发成功之后需要硬件厂家支持,但是硬件厂家没同意(因为Oak没人用,硬件厂商怕支持了没人买)Oak直接凉了
1995年互联网蓬勃发展,那时候需要一种语言可以在网络上传输,并且给页面增加动态效果,Green小组成员发现Oak可以完成这样的要求,并且当初设计的时候这门语言也比较小适合在网络上传输,所以对Oak语言一顿修改,重命名为Java,并且在Sun World大会上亮相被认可(IBM,微软,HP..)
1996年1月 Sun公司发布了Java的第一个开发工具包（JDK1.0）
1997年2月 JDK1.1
1999年6月 JDK1.2
	J2ME（Java2 Micro Edition Java2平台的微型版）应用于嵌入式、无线领域
	J2SE（Java2 Standard Edition Java2平台的标准版）应用于桌面环境,现在已经被Android应用和IOS应用取代
	J2EE（Java2 Enterprise Edition Java2平台的企业版）应用于基于Java的应用服务器,以后的学习方向
2000年5月 JDK1.3、JDK1.4
2004年9月 JDK5(从原来的1.x版本修改成x版本) 里程碑版本,增加了很多实用功能
2005年6月 JDK6
2009年    甲骨文(Oracle)公司宣布收购Sun
2011年    JDK7
2014年    JDK8(JDK8版本开始部分收费 JDK8u211及以上版本开始收费,以下版本依然免费) 当前国内使用的版本
2017年9月 JDK9 收费
2018年3月 JDK10 收费
...
2021年    JDK16 收费
2021年    JDK17 免费
...
          JDK19 免费
2023年	JDK20(筹备中...)
...        
```

<!-- more -->

## 第二章 Java语言优势

### 第1节 Java语言应用方向

```xml
Java语言的职业方向
1. Android系统应用早期是用Java语言开发的,当然现在也可以使用,不过现在有替代语言Kotlin
2. 很多大数据相关框架都是使用Java语言开发的,比如hadoop，Hbase，Elasticsearch等
3. 在应用领域很多软件的后台服务都是基于Java开发
	3.1 电商领域(淘宝、京东...)
	3.2 金融领域(支付宝、银行...)
	3.3 软件工具(Eclipse...)
	3.4 国内的各个领域(政府、金融、保险、医疗、教育...)Java占着举足轻重的地位

打开我们的手机,里面的APP软件大部分后台都是Java语言编写的.
```

### 第2节 Java语言自身特点
```xml
1. 简单性
2. 面向对象
3. 分布性
4. 健壮性
5. 安全性
6. 跨平台
...
```
## 第三章 Java语言的环境搭建

```xml
Java语言属于高级语言,采用Java语言编写的程序,计算机是不能直接运行的,这时候需要一个语言翻译(编译)工具,翻译成机器可以识别的机器码才可以.

Java官方提供了JDK(Java Development Kit)Java开发工具包,工具包中包含了开发Java语言的各种工具(包含语言编译工具)以及运行Java软件的环境等
```

* 下载地址

```xml
https://www.oracle.com/java/technologies/downloads/
```

* 安装

```xml
傻瓜式安装
注意: 安装路径不能有中文,不能有空格,不需要额外安装jre(Java运行时环境)
```

* 安装目录介绍

<table>
	<tr>
		<td> <img src="https://note.youdao.com/yws/api/personal/file/WEB46e65b155fd784526a889d04673cf545?method=download&shareKey=9c57ce9c71cabbbfb0ad318b8d164027"> </td>
	</tr>
	<tr>
		<td> <img src="https://note.youdao.com/yws/api/personal/file/WEB6fcb67dc62b4622f59bb2d8c755176c4?method=download&shareKey=43b8f42212b4cf094cfa7cc9035e01b0"> </td>
	</tr>
</table>

* 环境变量配置

```xml
1. 设置变量 JAVA_HOME=JDK安装路径,精确到bin目录的上一级
2. 将JAVA_HOME添加到Path路径下 %JAVA_HOME%\bin;

注意:
1. JAVA_HOME 必须全部大写
2. JAVA_HOME 两个单词中间是下划线而非减号
3. %XXX% 百分号是windows系统中取变量值的一种语法 
```

* 测试

```xml
在DOS命令行窗口中执行 java -version 命令. 如果打印出jdk的版本,说明安装成功.
```

## 第四章 第一个Java软件编写

### 第1节 开发工具介绍

```xml
Windows自带的文本文档              免费(功能较少)
notepad++ (后改名为 notepad-plus)  免费
SublimeText                       收费
Eclipse                           免费
IntelliJ IDEA                     收费
...
```

### 第2节 Java代码编写步骤

```xml
1. 创建一个文本文件,文件的格式以.java结尾(这里为了能看清你电脑上每一个文件的格式,需要将文件后缀名设置出来.)
2. 文件的名字理论上可以随意命名,但是这里强调不能使用中文,只能使用 0-9、a-z、A-Z、_、$ 不能使用其他
3. 文件名字不能以数字开头
4. 在给文件起名字时,英文单词的首字母必须大写,多英文单词的时候首字母也一定要大写 eg: XxxYyyZzz
```

### 第3节 代码编写

```java
public class HelloWorld{
	public static void main(String[] args){
		System.out.println("HelloWorld");
	}
}
```
## 第五章 软件的运行流程

```xml
1. 首先Java语言是高级语言,高级语言编写的代码是不能直接被计算机解析和运行的(计算机只认识0和1)
2. 所以首先就是对我们编写的代码进行处理(编译),处理成可以被计算机认识的形式
3. 编译 javac XxxYyyZzz.java 将.java的源文件编译成XxxYyyZzz.class字节码文件
4. 运行 java XxxYyyZzz  -- 将其加载到JVM虚拟机的内存中由CPU解析和运行
```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB4cf28e123fcaf24959991d947af085f6?method=download&shareKey=9a37def37a7dbd7751c14799b6abd74c">
		</td>
	</tr>
</table>

## 第六章 Java语言注释
```xml
什么是注释: 标注计算机代码,起到解释说明作用,给程序员(软件开发者)看的.
```
```xml
//单行注释
/* 
 * 多行注释
 */
/**
 * 文档型注释/多行注释/Java语言独有的
 */
jdk提供了对文档注释的操作使用javadoc命令可以生成java文档
eg: 
javadoc  -d C:\Users\Administrator\Desktop\d  -encoding utf-8  HelloWorld.java
```

---

<div style="font-size:10px;text-align:center;"><strong style="color:blue;">※※※※※※</strong> 大道至简(~(@^_^@)~) <strong style="color:blue;">※※※※※※</strong></div>

---