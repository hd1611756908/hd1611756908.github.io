---
title: 04-JavaScript笔记
categories: 前端
tags: ["前端","html","css","js","javascript","ES6"]
---

```xml
作者 : 夜泊1990
邮箱 : hd1611756908@163.com
企鹅 : 1611756908
鹅群 : 948233848
博客 : https://hd1611756908.github.io/
破站 : https://space.bilibili.com/514155929/
```


## 第一章 JavaScript概述

### 第1节 JavaScript简介
```xml
1. JavaScript(简称"JS") 一种动态类型、弱类型、基于原型的客户端脚本语言
2. JS是一门纯面向对象的语言
3. JS的作者是Brendan Eich(布兰登·艾奇)
4. JS的解释器被称为JavaScript引擎,为浏览器的一部分,最早是在HTML网页上使用,用来给HTML网页增加动态功能(现在也是其主要功能)
```

<!-- more -->

### 第2节 JS的历史

```xml
JavaScript诞生于1995年,由Netscape公司的Brendan Eich,在网景导航者浏览器上首次设计实现 
Netscape在最初将其脚本语言命名为LiveScript,后来Netscape与Sun合作,借其名气改名JavaScript
```

### 第3节 ECMAScript规范与JavaScript

```xml
在当年不是JS一家独大,当时还有其他的JS语言,多种JS语言没有统一的规范.这让JS编译器比较头疼.

举例: 
    A编写了一套代码用的JavaScript语言 
    B编写了一套代码用的ActionScript语言
    由于两种JS语言没有强制性的约束,这就使得浏览器编译器需要对两套代码的语法都需要兼容,只兼容A的话B代码不能运行,只兼容B那么A代码不能运行
    对于浏览器来说只做JavaScript语法兼容,那么就会失去ActionScript语言的客户,所以需要对各个JS语言进行兼容,特别头疼.

1996年11月 JavaScript 的创造者 Netscape 公司,决定将 JavaScript 提交给标准化组织 ECMA,希望这种语言可以成为国际标准.
1997年 ECMA 发布262号标准文件（ECMA-262）规定了浏览器脚本语言的标准,并将这种语言称为 ECMAScript,这个版本就是1.0版

从那时开始JS有了标准ES1.0(那时候不这么叫),要求所有的JS语言都需要按照此标准进行设计,否则浏览器不做兼容.
ECMAScript与JavaScript的关系,前者是后者的规范,后者是前者的实现.ECMAScript当时还有两个比较出名的小弟JScript和ActionScript.


ECMAScript版本发展

1997年    ES 1.0 发布
1998年    ES 2.0 发布
1999年    ES 3.0 发布
2007年    ES 4.0草案发布(各大厂商意见分歧,未通过)
2008年    发布ES 3.1 并改名为ECMAScript 5
2009年    ES 5.0 正式发布
2011年    ES 5.1 发布
2013年    ES 6 草案发布
2015年    ES 6 正式发布 并且更名为 ECMAScript 2015
2019年    ES 10 发布
...

每一个版本的ES都会新增很多JS语法,具体请查找资料
```

## 第二章 JS的编写位置

```xml
C语言编写在一个后缀是.c的文件中
Java语言编写在后缀是.java的文件中
Python编写在后缀是.py的文件中
你猜对了JS语言编写在后缀是.js的文件中

当前JS语言又和其他的语言不太相同,它不仅仅可以编写在.js文件中,它还可以嵌入到html文件中
```

### 第1节 行内式

```xml
JS写到HTML标签中

例如: <button onclick="alert('行内式')">行内式</button>
```

### 第2节 内嵌式(开发中常见)

```xml
将JS代码写到HTML文件中的head/body标签中,使用方式和css类似,将JS代码写到script标签中

例如:
<script type="text/javascript">
    alert('内嵌式');
</script>
```

### 第3节 外部式(开发中常见)

```xml
在外部创建一个以.js结尾的文件,然后在当前HTML文件中使用script标签引入

例如:
0. 创建一个 xxx.js 文件
1. 在head标签中引入 <script src="xxx.js"></script>
2. 在body标签中引入 <script src="xxx.js"></script>
```

## 第三章 JS入门之HelloWorld

```html
//基本所有的计算机语言在学习之前都会编写一个入门的代码,输出HelloWorld字符串

//点击button按钮,在浏览器弹窗HelloWorld/或者是在浏览器的控制台输出HelloWorld

//方式一(行内式)

<button onclick="alert('HelloWorld')">HelloWorld</button>

//方式二(内嵌式)

<button id="btn">HelloWorld</button>
<script type="text/javascript">
    window.onload=function(){
        var btn = document.getElementById("btn");
        btn.onclick=function(){
            alert("HelloWorld");
        }
    }
</script>

//方式三(外部式): 外部式代码和内嵌式代码一模一样,就是将JS代码写到外部的JS文件中.
```
## 第四章 JS的基础语法

### 第1节 变量的声明

```js
变量的概念: 内存中的一块存储单元,该单元有自己的名字和类型,变量需要先声明后使用.

变量的声明: JS使用关键字 var 进行变量声明.
例如: 
    var a=100; //声明一个整数类型变量
    var b="Hello"; //声明一个字符串类型变量
    var c=true; //声明一个布尔类型变量
    var d=1.01; //声明一个小数类型变量

在ES6版本中新增了两个关键字进行变量声明 let和const

例如: 
    let e=200;
    let f="world";
    let g=false;
    let h=2.11;

    const i=300;
    const j="year";
    const k=true;
    const m=3.11;

var、let、const 有什么区别呢?

const: 声明一个常量,值不能被修改
var 和 let :作用域不同,var 作用域大于 let

例如:
    const c=300;
    c=400; //报错,常量不能进行二次赋值

    <script>
        {
            var a=100; //作用范围全局
            let b=200; //作用范围当前大括号
        }
        console.log(a); //输出100
        console.log(b); //b未定义
    </script>


在ES6版本中还新增了一种全新的赋值方式 destructuring(解构[jiěgòu]赋值),语法格式如下
例如:
    var [a,b,c]=[1,'word',true];
    console.log(a);//1
    console.log(b);//word
    console.log(c);//true

    var [a,,c]=[1,3];
    console.log(a);//1
    console.log(c);//undefined

    解构赋值要求等号左右两侧要一一对应,否则可能就会报undefined

    解构不仅可以用于数组还可以用于对象
例如:
    var {key1,key2}={key1:1001,key2:'李雷'}
    console.log(key1);//1001
    console.log(key2);//李雷
```

### 第2节 数据类型介绍

变量 | 解释 | 示例
---|---|---
String |字符串: 一串文字,字符串的值必须用引号(单引号或者双引号)括起来 | var s='李雷'
Number |数字(整数/小数) | var x=10; var y=1.0;
Boolean |布尔值: 真或者假. 真:true;假:false | 
Array |数组: 存储一组数据 | var arr=[1,2,'Polly',[3,4,5]];
Object |对象: 万事万物皆对象,一切都可以保存到变量中| var obj1={}; var obj2 = new Object();

```js
JS进行变量声明时是有类型的,但是它的类型声明不像Java语言这种强类型语言这么明确.可以使用JS提供的函数查看当前声明变量的数据类型.

var a=100;
var b=true;
var b="JS";

可以使用typeof函数获取各个变量的数据类型

例如: 
	var aa =  typeof(a);
	console.log(aa); //Number
	var bb =  typeof(b);
	console.log(bb);//Boolean
	var cc =  typeof(c);
	console.log(cc);//String
```

### 第3节 对象和数组

#### 3.1 对象类型

```js
对象的声明和初始化

JS创建对象 方式一:

//创建对象
var obj = {};
//给对象赋值
obj.id=100;
obj.name="李雷";
//控制台输出
console.log(obj);//输出对象
console.log(obj.name);//输出对象属性
//修改属性值
obj.id=200;
obj.name="韩梅梅";
//控制台重新输出
console.log(obj);//输出对象

JS创建对象 方式二:

//创建对象
var obj = new Object();
//给对象赋值
obj.id=300;
obj.name="张三";
//控制台输出
console.log(obj);//输出对象

```

#### 3.2 数组类型

```js
数组的声明和初始化

数组的创建方式一:

//创建数组对象(可以是空数组,也可以直接在数组中赋值)
var arr1=[];//或者var arr1=['AAA',true,100];
//给数组对象赋值
arr1[0]="BBB";
arr1[1]=100.12;
arr1[2]=true;
//输出
console.log(arr1);

数组的创建方式二:

var arr2=new Array('AAA',123);
//给数组对象赋值
arr2[0]="BBB";
arr2[1]=100.12;
arr2[2]=true;
//输出
console.log(arr2);

数组的创建方式三:

var arr3=new Array(10);
//给数组对象赋值
arr3[0]="BBB";
arr3[1]=100.12;
arr3[2]=true;
//输出
console.log(arr3);

//注意: 当new Array(10)构造器里面值赋一个整数值或者不赋值时,代表的是数组的长度,当大于一个值是表示数组的内容,构造器内输入数据的类型可以是任意类型
```

#### 3.3 数组的常见操作函数

```js
1. push() : 向数组尾部添加一个元素
2. pop()  : 从数组尾部弹出一个元素,并且返回弹出的元素
3. shift(): 头部删除一个元素
4. unshift(): 头部添加一个元素
5. concat([数组]): 数组连接  [1,2,3].concat([4,5]) [1,2,3,4,5]
6. join(): [1,2,3].join("-") 1-2-3
```

### 第4节 运算符

```xml
运算符: 
算数运算符: + - * / %  += -= *= /= %=  ++ --
关系运算符: < > >= <= == != ===(严格等于,既比较值又比较数据类型)
逻辑运算符: && || !
三目运算符: 表达式 ? 结果1:结果2
语法规则和Java基本相同,在进行除法运算时,因为没有整数类型和浮点类型的区分所以,不会取整

注意: 虽然js是一个弱类型语言,但是当遇到下面的运算时也需要类型转换.

var a=10;
var b="10";
var sum=a+b;//如果不将b进行类型转换,那么就会变成字符串拼接.

parseInt();将字符串转换成number

var sum=parseInt(b)+a;
console.log(sum);
```

### 第5节 分支结构和循环结构

#### 5.1 分支结构

```js

//if语句
if(true/false){
	//代码
}else if(true/false){
	//代码
}else{
	//代码
}

//switch流程控制
var a=2;
switch(a){
  case 1:
    /* 浏览器输出 */
    document.write("值为1");
    break;
  case 2:
    /* 控制台输出 */
    console.log("值为2");
    break;
  case 3:
    document.write("值为3");
    break;
  default:
     document.write("值不是3也不是2也不是1");
}

语法格式和Java语言基本相同
```

#### 5.2 循环结构

```js
for循环
while循环

break与continue关键字
- break用于结束循环
- continue用于结束本次循环

/*
    打印九九乘法表
*/ 
for(var i=1;i<=9;i++){
    for(j=1;j<=i;j++){
        document.write(i+"*"+j+"="+(i*j)+"&nbsp;&nbsp;&nbsp;&nbsp;");
    }
    document.write("<br>");
}

语法和Java相同
```

### 第6节 函数(方法)

#### 6.1 函数的创建和使用

```js

1. 函数的定义: 特定功能的代码集合,可以被反复调用.  函数定义采用关键字 function

1.1 函数定义方式一

语法格式:
	function 方法名(参数列表){
		//方法体
		return //如果有返回值使用return,如果没有返回值不加
	}
例如:
	function sum(a,b){  //有参
		return a+b;
	}
	function log(){ //无参
		console.log("Hello World...");
	}
	
	//函数调用(使用方法名)
	var s = sum(100,200);
	console.log(s);
	log();

1.2 函数定义方式二
	var sum=function(a,b){  //有参
		return a+b;
	}
	var log=function(){    //无参
		console.log("Hello World...");
	}
	//函数调用(使用变量名)
	var s = sum(100,200);
	console.log(s);
	log();


1.3 函数的使用扩展
	
	在JS中函数也是对象(万事万物皆对象),本质上和1、"Hello"、true、1.22 没有任何区别都是对象
	
	var obj = {};
	obj.id=1001;
	obj.name="Tom";
	
	//创建函数
	function add(x,y){
		return x+y;
	}

	//将函数赋给对象
	obj.add=add;
	console.log(obj);//输出当前对象查看内部
	//函数调用
	var result = obj.add(100,300);
	console.log(result);
	
	

ES6对函数的扩展(ES6版本新增函数入参可以直接赋初始化值)
	
	function sum(x=1,y="Hello"){ //定义函数时给参数赋初始化值
		return x+y;
	}
	var s = sum();//不给参数,使用默认值
	console.log(s);
	var c = sum("Hello "," World");//设置参数,覆盖默认值
	console.log(c);

```

#### 6.2 Class基础语法

```js
JS是一门纯面向对象的语言

JS语言中生成对象的传统方式(ES6版本之前)采用构造函数:
例如:
	function User(id,name){ //定义构造函数(其实就是函数)
		this.id=id;
		this.name=name;
	}
	
	//调用构造函数
	var u = new User(1001,"韩梅梅");
	console.log(u);//输出

JS语言生成对象最新方式(ES6方式)

ES6引入了类的概念(类似于C++和Java)

//创建类
class Student{
	//创建构造方法 constructor 是类的默认方法,构造方法只能有一个
	/* constructor(){//无参
		
	} */
	constructor(x,y){//有参
		this.x=x;
		this.y=y;
	}
	
	sum(x,y){ //普通方法
		return x+y;
	}
}
//创建对象
//var s = new Student();
var s = new Student(10,20);
s.sum(3000,4000);//调用其他方法
console.log(s);//输出

```

### 第7节 JS文档使用

```yaml
JavaScript这门语言内置了很多的对象和函数,这些对象可以通过文档来查询. 这里使用w3cschool这个文档
文档地址为: https://www.w3cschool.cn/javascript/ 

JS内置对象查询文档地址为: https://www.w3cschool.cn/javascript/js-objects.html
```

#### 7.1 JS内置对象Date

```js
时间对象: 
var myDate = new Date();
console.log(myDate);
console.log(myDate.getFullYear());//年
console.log(myDate.getMonth()+1);//月
console.log(myDate.getDate());//日
console.log(myDate.getHours());//时
console.log(myDate.getMinutes());//分
console.log(myDate.getSeconds());//秒
console.log(myDate.getDay());//星期日是0
console.log(myDate.toLocaleDateString());//日期
console.log(myDate.toLocaleTimeString());//时间
console.log(myDate.valueOf());//毫秒
```

#### 7.2 JS内置对象Math

```js
数学对象Math
console.log(Math.PI);
console.log(Math.abs(-1));//绝对值
console.log(Math.floor(4.99));//向下取整
console.log(Math.max(1,2,3,4));//最大值
console.log(Math.min(1,2,3,4));//最小值
console.log(Math.random());//0-1随机数
```

#### 7.3 JS内置对象Cookie

##### 7.3.1 cookie介绍
```xml
cookie英文单词饼干,小甜点的意思,在JS中他是一段可以保存不超过4KB的小型文本数据,保存在浏览器中
```

##### 7.3.2 cookie数据结构

```js
cookie是以key=value的形式存储

例如: username=tom
```

##### 7.3.3 cookie怎么使用

```xml
1. 向浏览器中保存数据: document.cookie="username=tom";
2. 从浏览器中获取cookie数据 var c = document.cookie;
```

##### 7.3.4 cookie的其他参数

```xml
1. expires 设置cookie的过期时间,时间格式必须为UTC 或 GMT 时间格式
2. path 设置cookie的存储路径默认为 /
```

##### 7.3.5 设置和获取cookie案例

```xml
1. 设置cookie

//创建cookie
//创建过期时间
var d = new Date();//当前时间
//根据当前时间向后推一天作为过期时间
d.setTime(d.getTime()+2*24*60*60*1000);//向后推2*24*60*60*1000毫秒(2天)
//创建cookie格式,设置数据,过期时间,以及保存位置
var cookie_str="username=tom1;expires="+d.toGMTString()+";path=/";
document.cookie=cookie_str;
console.log(cookieStr);

2. 获取cookie

//获取cookie
var c = document.cookie;
console.log(c);
```

## 第五章 正则表达式

### 第1节 正则表达式介绍

```yaml
1. 正则表达式是由一个字符序列形成的搜索模式
2. 当你在文本中搜索数据时，你可以用搜索模式来描述你要查询的内容
3. 正则表达式可以是一个简单的字符，或一个更复杂的模式
4. 正则表达式可用于所有文本搜索和文本替换的操作
```

### 第2节 正则表达式语法

```yaml
/正则表达式主体/修饰符(可选)
```

### 第3节 正则表达式的对象和方法


* RegExp对象

方法 | 描述
---|---
exec | 检索字符串中指定的值。返回找到的值,并确定其位置
test | 检索字符串中指定的值。返回 true 或 false

* String对象

方法 | 描述
---|---
search | 检索与正则表达式相匹配的值
match | 找到一个或多个正则表达式的匹配
replace | 替换与正则表达式匹配的子串
split | 把字符串分割为字符串数组


### 第4节 正则表达式的使用


* 构建正则表达式的特殊字符

```yaml
修饰符: 用于执行区分大小写和全局匹配
```

修饰符 | 描述
---|---
i | 执行对大小写不敏感的匹配
g | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
m | 执行多行匹配

```yaml
方括号: 用于查找某个范围内的字符
```

表达式 | 描述
---|---
[abc] | 查找方括号之间的任何字符
[^abc] | 查找任何不在方括号之间的字符
[0-9] | 查找任何从 0 至 9 的数字
[a-z] | 查找任何从小写 a 到小写 z 的字符
[A-Z] | 查找任何从大写 A 到大写 Z 的字符
[A-z] | 查找任何从大写 A 到小写 z 的字符
(red|blue|green) | 查找任何从 0 至 9 的数字

```yaml
元字符(Metacharacter): 是拥有特殊含义的字符
```

元字符 | 描述
---|---
. | 查找单个字符,除了换行和行结束符
\w | 查找单词字符
\W | 查找非单词字符
\d | 查找数字
\D | 查找非数字字符
\s | 查找空白字符
\S | 查找非空白字符
\b | 匹配单词边界
\B | 匹配非单词边界
\0 | 查找 NULL 字符
\n | 查找换行符
\f | 查找换页符
\r | 查找回车符
\t | 查找制表符
\v | 查找垂直制表符
\xxx | 查找以八进制数 xxx 规定的字符
\xdd | 查找以十六进制数 dd 规定的字符
\uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符

```yaml
量词: 用于表示重复次数的含义
```

量词 | 描述
---|---
n+ | 匹配任何包含至少一个 n 的字符串
n* | 匹配任何包含零个或多个 n 的字符串
n? | 匹配任何包含零个或一个 n 的字符串
n{X} | 匹配包含 X 个 n 的序列的字符串
n{X,} | X 是一个正整数。前面的模式 n 连续出现至少 X 次时匹配
n{X,Y} | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配
n$ | 匹配任何结尾为 n 的字符串
^n | 匹配任何开头为 n 的字符串
?=n | 匹配任何其后紧接指定字符串 n 的字符串
?!n | 匹配任何其后没有紧接指定字符串 n 的字符串

* 常见正则表达式的构建

```js
1. 匹配手机号

var phone=/^[1][3,4,5,7,8][0-9]{9}$/;

^: 匹配输入的开始
[1]: 第1位以1开始
[3,4,5,7,8] : 第2位可以是3,4,5,7,8  这时候手机号前两位就出来了 13,14,15,17,18 如果以后手机号还有别的开头的比如说19,那么在第二个中括号中添加9即可
[0-9]{9}$ : {9}表示前面还有9位可以是0-9任意; $: 表示结尾

2. 匹配身份证

//校验身份证15位或者18位的正则,如果为18位后一位可能是数字或者x
var cardId=/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
```

* 正则表达式的使用

```js
1. 检索字符串中某一个子串是否存在

//使用string对象中的方法
var str="abCDef";
//返回cd所在字符串中的下标值,如果不存在返回-1  i: 表示不区分大小写,可以省略
var f = str.search(/cd/i);
console.log(f);

2. 检索一个字符串是不是以数字开头

//使用JavaScript对象
var reg=/^\d/; //以数字开头
var f = reg.test("1abc");//如果以数字开头返回true
console.log(f);

3. 检索一个字符串是不是正确的手机号

var phone=/^[1][3,4,5,7,8][0-9]{9}$/;
var f = phone.test("19252565897");
console.log(f);

4. 检索一个字符串是不是一个正确的身份证号

var cardId=/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;
var f = cardId.test("23230319966582256x");
console.log(f);
```

## 第六章 JS的严格模式

```js
//传统方式
<script>
			
	/*
		传统方式
	*/ 
	x=100;//变量可以不需要声明直接使用,但是不会报错
	console.log(x);
</script>


//严格模式
<script>
			
	/*
		严格模式
	*/
	"use strict" //添加这句话浏览器编译器会执行严格模式
	x=100;//变量不声明会报错
	console.log(x);
</script>



严格模式是在ES5中新增,如果使用旧版本的浏览器,这条语句会被忽略,在支持严格模式的浏览器中会执行.

严格模式会让有些原本执行的正常的JS代码出现问题,具体详细了解严格模式请查阅资料,这里不再赘述.
```

## 第七章 JS高级(HTML DOM)

### 第1节 DOM介绍

```xml
1. 当网页被加载时,浏览器会创建页面的文档对象模型DOM(Document Object Model),将整个.html文件读成一个对象(document对象)
2. DOM是一项W3C(World Wide Web Consortium)标准,它允许程序和脚本动态地访问、更新文档的内容、结构和样式
3. W3C: 万维网联盟,又称W3C理事会,是国际最著名的标准化组织,W3C最重要的工作是制定、发展 Web规范
```

### 第2节 DOM树介绍

```xml
DOM树: document对象里描述了整个.html文件(包含各个前端标签),里面的各个标签之间产生了各种关系,这种关系就像树形结构
```

<table>
	<tr>
		<td> <img src="https://note.youdao.com/yws/api/personal/file/WEBb7d88519df07d390b933a461aa3ee97f?method=download&shareKey=9552e94a6ef8323f3c22e7e97a0eb25d"> </td>
	</tr>
</table>

### 第3节 DOM对象的获取

```xml

通过document对象调用如下方法和属性,那么document对象怎么获取呢？

document对象在浏览器加载和解析.html文件时生成,每一个.html文件都会生成一个新的document对象,不需要我们手动创建,直接使用即可.

一、获取元素节点:
1. getElementById(): 通过id属性获取一个元素节点对象
2. getElementsByTagName(): 通过标签名获取一组元素节点对象
3. document.getElementsByClassName():通过类名获取一组元素节点对象
4. getElementsByName(): 通过name属性获取一组元素节点对象

二、获取元素节点的子节点
1. getElementsByTagName(): 方法，返回当前节点的指定标签名后代节点
2. childNodes: 属性，表示当前节点的所有子节点
3. firstChild: 属性，表示当前节点的第一个子节点
4. lastChild: 属性，表示当前节点的最后一个子节点

三、获取父节点和兄弟节点
1. parentNode: 属性，表示当前节点的父节点
2. previousSibling: 属性，表示当前节点的前一个兄弟节点
3. nextSibling: 属性，表示当前节点的后一个兄弟节点
```

### 第4节 DOM对象的详解
```xml
DOM对象不仅包含document对象,还包括document对象中的所有的子对象(每一个标签(元素)对象,标签对象也称之为DOM对象).

标签(节点/元素): Node——构成HTML文档最基本的单元
常用节点分为四类:
    – 文档节点:整个HTML文档(document)
    – 元素节点:HTML文档中的HTML标签 
    – 属性节点:元素的属性
    – 文本节点:HTML标签中的文本内容
```

<table>
	<tr>
		<td><img src="https://note.youdao.com/yws/api/personal/file/WEBbd6782fb7a7b80b8efe32f11d0f57010?method=download&shareKey=753112247d53a9600488a0a5e14a7ca9"> </td>
	</tr>
</table>

```xml
1、文档节点(document)
   - 1.1 文档节点document,代表的是整个HTML文档,网页中的所有节点都是它的子节点
   - 1.2 document对象作为window对象的属性存在,我们不用获取可以直接使用.
   - 1.3 通过该对象我们可以在整个文档中查找节点对象,并可以通过该对象创建各种节点对象
2、元素节点(Element)
   - 2.1 HTML中的各种标签都是元素节点,这是我们最常用的一个节点
   - 2.2 浏览器会将页面中所有的标签都转换为一个元素节点,我们可以通过document的方法来获取元素节点
        -- 比如: 
		
		<!--HTML代码-->
		<p id="pId">这是一个P标签</p>
        //JS代码,通过id获取元素节点
        var pEle = document.getElementById("pId");
		console.log(pEle);
		
3、文本节点(Text)
   - 3.1 文本节点表示的是HTML标签以外的文本内容,任意非HTML的文本都是文本节点
   - 3.2 它包括可以字面解释的纯文本内容
   - 3.3 文本节点一般是作为元素节点的子节点存在的
   - 3.4 获取文本节点时,一般先要获取元素节点.在通过元素节点获取文本节点
        -- 例如:
		
		<!--HTML代码-->
		<p id="pId">这是一个P标签</p>
        //JS代码,通过id获取元素节点
        var pEle = document.getElementById("pId");
		console.log(pEle);
		//获取文本节点
		var text = pEle.firstChild;
		console.log(text);
		
4、属性节点(Attr)
   - 4.1 属性节点表示的是标签中的一个一个的属性，这里要注意的是属性节点并非是元素节点的子节点，而是元素节点的一部分
   - 4.2 可以通过元素节点来获取指定的属性节点
        -- 例如:
		
		<!--HTML代码-->
		<p id="pId">这是一个P标签</p>
        //JS代码,通过id获取元素节点
        var pEle = document.getElementById("pId");
		console.log(pEle);
		//获取属性节点
		var attr = pEle.getAttributeNode("id");
		console.log(attr);	
```

### 第5节 DOM事件

#### 5.1 事件简介

```xml
事件:文档或浏览器窗口中发生的一些特定的交互瞬间.
JavaScript 与 HTML 之间的交互是通过事件实现的.

事件三要素:
事件源: 被触发的物体
事件类型: 用户的行为,单击，双击，移入，移出，拖拽，滑屏
事件处理函数: 执行的代码
```

#### 5.2 事件类型

```xml
单击 onclick
鼠标移入 onmouseover
鼠标移出 onmouseout
获取焦点 onfocus
失去焦点 onblur

...
```

#### 5.3 事件绑定的方式

```html
1. 方式一

HTML:
<button id="btn" >JavaScript事件绑定</button>

JS:
var btn = document.getElementById("btn");
btn.onclick=function(){
	alert("+++++++");
}

2. 方式二

HTML:
<button id="btn" >JavaScript事件绑定</button>

JS:
var btn = document.getElementById("btn");
// click:不带on的事件类型
// function:事件处理函数
// false:指定事件是否在捕获或冒泡阶段执行
btn.addEventListener("click",function(){
	alert("=====");
},false);
```

### 第6节 DOM对象创建以及内容修改

#### 6.1 元素节点的创建和修改

```html
1、创建元素节点
    document.createElement("标签名");
2、创建文本节点
    document.createTextNode("北京");
3、删除节点
    父节点.removeChild(子节点);
4、替换节点
    父节点.replaceChild(新节点,旧节点);
5、插入节点
   5.1 父节点.appendChild(子节点);
   5.2 父节点.insertBefore(新节点,旧节点);
```

#### 6.2 设置/获取元素内容

```html
innerText:设置元素内容,原样输出
innerHTML:设置元素内容,如果设置的是一个标签元素会被浏览器解析

HTML:
<p id="pId">ppppp</p>

js:
var pIdEle = document.getElementById("pId");
pIdEle.innerHTML="<h1>看这里,我变~~~</h1>";
```

### 第7节 定时器

```js
//设置定时器1:时间到了之后定时器执行一次然后结束
setTimeout("参数1","参数2");
参数1: 被定时的函数
参数2: 定时时间(多久执行一次函数 ms)
eg: setTimeout(function(){console.log("=====")},3000);

//设置定时器2:根据时间循环执行函数
setInterval("参数1","参数2");
参数1: 被定时的函数
参数2: 定时时间(多久执行一次函数 ms)
var timeId = setInterval(function(){console.log("=====")},3000);

//取消定时器
//可以指定要取消的哪个定时器timeId,为开启的定时器的返回值
window.clearInterval(timeId);
```

### 第8节 JS地址跳转

```html
前端进行目标地址跳转的方式有两种
1. 标签跳转(a标签/form标签)
	
	<a href="目标地址">跳转</a>
	<form action="目标地址"></form>
	
2. JS方式跳转
	
	location.href="目标地址"
	
	location.href：如果赋值,表示跳转地址;如果不赋值表示获取当前页面在浏览器地址栏中的地址.
```

### 第9节 JS设置元素的CSS样式

```html
使用JS设置元素的CSS样式
方式一:
HTML:
<p id="pId">ppppp</p>
JS:
var pId = document.getElementById("pId");
pId.onclick=function(){
	pId.style.color="red";
}

方式二:
HTML:
<p id="pId">ppppp</p>

JS:
var pId = document.getElementById("pId");
pId.onclick=function(){
	pId.style.cssText="color:red;font-size: 30px;";
}
```

## 第八章 练习(轮播图)

<table>
	<tr>
		<td> <img src="https://note.youdao.com/yws/api/personal/file/WEB68fa9a6549908f8c6da39fe4696e9cd4?method=download&shareKey=36349ac6d5cdead35054e73d6906d532"> </td>
	</tr>
</table>

```xml
素材地址:

链接: https://pan.baidu.com/s/1e7D1-zLDB9ZvDzzP2wRunA
提取码: jjq5

素材来源 https://www.readnovel.com/
```


```bat
本课件不适合0基础人员,以Java程序员的视角学习JS.并且不会深入JS语言的学习,只是起到快速入门的效果.
```