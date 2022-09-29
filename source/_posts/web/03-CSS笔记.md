---
title: 02-CSS笔记
categories: 前端
tags: ["前端","html","css","js","javascript"]
---

```xml
作者 : 夜泊1990
邮箱 : hd1611756908@163.com
企鹅 : 1611756908
鹅群 : 948233848
博客 : https://hd1611756908.github.io/
破站 : https://space.bilibili.com/514155929/
```

## 第一章 CSS简介
```xml
CSS全称为"层叠样式表"(Cascading[kæˈskeɪdɪŋ] Style Sheets),它主要作用是定义HTML内容在浏览器内的显示样式,如文字大小、颜色、字体加粗等
```

<!-- more -->

## 第二章 CSS编写位置
```xml
给一个标签元素添加样式的方式有三种:

第一种: 直接在标签元素上添加样式,所有的HTML标签都有style属性,这个属性是给标签添加样式的基本属性. 例如: <p style="color:red;">一段话</p>
第二种: 在HTML模板的head标签中,声明一个style标签,语法如下:
	<style type="text/css">
		/* 编写CSS代码(这是CSS注释) */
	</style>
第三种: 创建css文件的方式,在外部创建一个以.css结尾的文件.例如xxx.css,然后在HTML文件的head标签中使用link标签引入,语法如下:
	<link rel="stylesheet" type="text/css" href="xxx.css"/>
```
## 第三章 CSS选择器

### 第1节 HTML标签详解

```xml
CSS的功能是给指定的标签添加样式,那么问题来了,我们怎么定位到标签呢?给标签添加样式的前提首先是定位标签,然后在添加样式.

要想定位一个HTML标签,那么我们需要详细了解一下 任意一个标签是由什么组成的.

这里以p标签为例

<p>一段话</p>

上面这个p标签有标签名[p]以及标签内容[一段话],名字是固定不变的,内容是可变的.如果要用名字和内容定位标签,那么明显只能用名字来定位.

所以定位标签的第一个属性: 标签名,通过名字找到标签,然后添加要是

除了名字之外还有其他方式可以定位一个标签元素. 继续往下看↓↓↓↓↓↓

<p id="pId" class="pClass">一段话</p>

HTML的每一个标签还有一个固有属性,例如 id属性、class属性,这两个属性是HTML标签的通用属性,任意标签都有这两个属性,可以使用这两个属性定位标签.

id和class属性属于通用属性任意标签都有,还有一些属性是特有属性,不是每一个标签都有的;例如img的src属性,a标签的href属性等,后面详细介绍.
```

### 第2节 CSS选择器语法 

<table>
	<tr>
		<td style="width:50%;text-align:center;">
			<img src="https://note.youdao.com/yws/api/personal/file/54EB0418DB9F416CAEDFFCAEEA285495?method=download&shareKey=a8b5772a67c00e908f2797800651f2aa">
		</td>
		<td style="width:50%;">
			<strong style="color:red;font-size:12px;">选择符</strong> : <span style="font-size:12px;">又称选择器,指明网页中应用样式规则的元素(标签)</span><br/>
			<strong style="color:red;font-size:12px;">{}</strong>   : <span style="font-size:12px;">声明,设置元素样式,格式为key:value键值对,多个key:value使用分号分割</span>
		</td>
	</tr>
</table>

#### 2.1 选择器的介绍

<pre>
CSS样式定义由两部分组成,形式如下:
<span style="color:red;">选择器</span>{
    样式;
}
在<span style="color:red;">{}</span>之前的部分就是<span style="color:red;">"选择器"</span>,<span style="color:red;">"选择器"</span>指明了<span style="color:red;">{}</span>中的<span style="color:red;">"样式"</span>的作用对象,也就是<span style="color:red;">"样式"</span>作用于网页中的哪些标签
</pre>

#### 2.2 选择器的分类

```html
1、标签选择器 语法: p{}
	选择器就是标签名,例如给p标签添加样式,案例如下:
	
	p{
		/* 给所有p标签的文本内容设置成红色 */
		color: red;
	}
	
2、类选择器  语法: .className{}
	HTML中的所有标签都有class属性,可以设置class的类名,多个标签可以设置相同的class类名,案例如下:
	
	<div class="container">我是一个div标签我的类名为container</div>
	.container{
		color: red;
	}
	注意事项: 类选择器类名前一定要加一个点(.)这是类选择器的标记语法.并且类选择器在class上可以命名多个,用空格分开.
	
3、ID选择器 (全局唯一) 语法: #id{}
   HTML中的所有标签都有id属性,可以设置id的名称,在同一个html文件中id的名称是唯一的,不能有两个标签的id名称相同,案例如下：
   
   <div id="show">我是一个div标签,我的id属性值为show</div>
   #show{
   	color: red;
   }
   注意事项: id选择器id属性名前一定要加一个#这是id选择器的标记语法.
   
4、子选择器 > (表示第一代子元素)    语法:  #id>p{}  
   HTML标签是可以嵌套使用的,嵌套的HTML标签之间就构成了子父类的关系,这时候我们如果知道父标签,就可以直接通过父标签使用子选择器找到他的儿子. 案例如下:
   
   <div id="box">
   	<div>
   		我是div1
   		<p>我是p标签</p>
		<div>我是div2</div>
   	</div>
   </div>
   给id是box的div下的div元素(大于号左右两侧可以是任意选择器,不局限与id和标签选择器),设置宽度和高度
   #box>div{
   	/* 只给了子div添加了样式,孙子的没有添加上 */
   	height: 100px;
   	width: 100px;
   }
5、后代选择器   语法:  #id p{}
   后代选择器和子类选择器类似,他是将某一个元素标签下的所有的目标元素添加样式,不论是儿子还是孙子等 案例如下:
   
   <div id="box">
   	<div>
   		我是div1
   		<p>我是p标签</p>
   		<div>我是div2</div>
   	</div>
   </div>
   #box div{
   	/* 给id为box的后代中的所有div添加样式 */
   	height: 100px;
   	width: 100px;
   }
   
5、通用选择器 *    语法:  *{}  星号(*)匹配所有的标签元素

*{
	font-size:14px;
}

6、伪类选择器(鼠标移入伪类) :hover    语法:  p:hover{}
   HTML所有标签元素都可以添加伪类选择器,伪类的意义是当鼠标悬停到当前标签下时,呈现的样式 案例如下:
   
   <div id="box"> 我是hover伪类 </div>
   #box:hover{
   	font-size: 100px;
   }
   
   其它伪类:
	   a:link {color:#FF0000;} /* 未访问的链接 */
	   a:visited {color:#00FF00;} /* 已访问的链接 */
	   a:active {color:#0000FF;} /* 已选中的链接 */

7、分组选择器 h1,h2,p  语法:  h1,h2,p{}
   分组选择器是给一组HTML标签添加样式,多个选择器使用逗号分隔
   
   <div id="box">我是div</div>
   <p class="pName">我是p标签</p>
   <span>我是span标签</span>
   <h1>我是h1标签</h1>
   #box,.pName,span,h1{
   	color: red;
   }
```

## 第四章 CSS优先级/继承/重要性

### 第1节 继承

```html
<div style="color: red;font-size: 100px;">
	<p> 测试继承关系 </p>
</div>
```

### 第2节 优先级

#### 2.1 权值

```html
1、标签的权值为1
2、类选择符的权值为10
3、ID选择符的权值最高为100
如果样式作用在一个标签上,根据权值来应用使用哪个样式(应用权值高的)


<style type="text/css">
	/* 
		如果权值相同,那么下面会覆盖上面的样式
		如果权值不同,谁大谁生效,和上下顺序无关
	*/
	p{color:red;}
	.first{color:green;}
</style>
<p class="first">三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```

#### 2.2 层叠

```xml
当作用在同一个标签上的样式权值相同时,那么遵循就近原则,谁离标签近,谁的样式就会生效
```

### 第3节 重要性

```html
有时候,在某些特殊情况下,在权值相同的情况下,要让某些样式的设置生效,这时候可以使用!important来设置他的最高权值

<style type="text/css">
	p{color:red !important;}
	p{color:green;}
</style>

<p class="first">三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```

## 第五章 CSS字体/段落/单位和值
### 第1节 字体/段落样式

```css
文字—字体:  
    font-family: "microsoft yahei";
文字—字号颜色:    
    font-size: 12px;
	color: #FF0000;
文字—粗体:
    font-weight: bold;
文字—斜体:
    font-style: italic;
文字-下划线
    text-decoration: underline;
文字-删除线
    text-decoration: line-through;
段落-缩进
    text-indent: 2em;
段落-行间距(行高)
    line-height: 1.5em;
段落-中文字间距、字母间距
    1、letter-spacing:50px;    中文或者是英文字母之间的间距
    2、word-spacing:50px;      英文单词之间的间距
段落-对齐
    text-align: left|center|right; 段落文本的左对齐|居中|右对齐
```

### 第2节 单位和值

#### 2.1 颜色值

```css
1、英文命令颜色
    p{color:red;}
2、RGB颜色
    p{color:rgb(133,45,200);}
3、RGBA颜色
    p{color:rgba(133,45,200,1);}
    rgba颜色有4位,最后一位为颜色的透明度,取值范围为0~1,1:不透明,0:全透明
4、十六进制颜色
    p{color:#00ffff;}
```

#### 2.2 字体大小/长度单位

```css
字体大小的单位一般我们前端使用px或者em表示
盒子高度/宽度的单位我们一般使用px或者%表示

px表示像素
像素的介绍: 咱们的显示器的屏幕是由很多点组成的,像素就是组成图像的最小单元,单位是px.

注意: 
1、em表示一个固定值,如果当前的文字的font-size为14px,那么1em=14px,例如当面的首行缩进,当前段落的字体大小为14px,那么首行缩进text-indent:2em,就是24px,正好是两个字体大小,一般取离他最近的字体作为参照.
2、%一般表示宽度,它会以父元素作为参照计算;如果父元素宽度200px,子元素宽度为50%,那么子元素宽度就为200*0.5=100px
```

## 第七章 CSS布局
### 第1节 HTML标签分类

```xml
在HTML标签学习过程中我们发现,有些标签独占一行,有些标签可以水平展示,如果我们想展示类似于下面的页面布局,这时候要了解各个标签的特性.

只有熟悉各个标签的特性,才能在前端页面上更好的布局(展示数据)
```

<table>
	<tr>
		<td><img src="https://note.youdao.com/yws/api/personal/file/WEBeea45743d21ec5accda92a9d88845fd8?method=download&shareKey=b64fa99fab94f1289547452ff94ac4ad"> </td>
	</tr>
</table>

```xml

前端标签按照特性分类: 1. 块状元素(独占一行,有宽高); 2. 内联元素(水平排列,无宽高); 3. 内联块状元素(水平排列,有宽高)

1. 块状元素: <div>、<p>、<h1>~<h6>、<ol>、<ul>、<table>、<form>
2. 内联元素: <a>、<span>、<em>、<strong>、<label>
3. 内联块状元素: <img>、<input>
```

### 第2节 元素转换

```css
display属性可以将一个元素转换成其它元素类型
	1、块状转内联
	2、内联转块状
	3、display: inline | block | inline-block;
```

### 第3节 元素的隐藏/显示

```css
display: none; 设置display属性为none,当前标签就会隐藏
```

### 第4节 CSS布局
```xml
前端提供了很多标签,本质上都可以进行布局,但是因为大多数标签都有自己明确的语义,不适合布局,布局一般使用没有语义或者语义不明确的标签.

HTML提供了两个特殊标签,来进行布局.一个为块状元素标签(div),一个为内联元素标签(span),并且前端为了更方便的进行布局,提出来盒模型概念
```

#### 4.1 CSS盒模型

```xml
盒模型是CSS布局的基石,它规定了网页元素如何显示以及元素间相互关系(用于前端布局),所有的块状元素都具有盒子模型的特点
```

<img src="https://note.youdao.com/yws/api/personal/file/91B9F1C034C24541BF159D28E6C89A8E?method=download&shareKey=33ac7d5675081a662dbe7c9a0c8aabc3">

#### 4.2 CSS的布局

##### 4.2.1 流动模型(Flow)

```xml
流动模型，流动（Flow）是默认的网页布局模式。也就是说网页在默认状态下的 HTML网页元素都是根据流动模型来分布网页内容的

流动模型的特点
1、块状元素独占一行
2、内联元素不会独占一行而是从左向右排列
```

##### 4.2.2 浮动模型(Float)

```xml
正常情况下块状元素独占一行,那么怎么能让块状元素并排显示呢?这时候就用到了浮动模型

实现浮动的css属性
float:left|right

高度塌陷问题处理
clear：left | right | both;
```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/76B722328420401FAEEA19783EF8265C?method=download&shareKey=f2c0a37470437223d93908bdabcd361c">
		</td>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/9CB359B9A07948D993B167934F42F98E?method=download&shareKey=94e2e67350ae7a943c36a48c98c532f0">
		</td>
	</tr>
</table>

##### 4.2.3 层模型(Layer)

* 绝对定位(position: absolute)

<table>
	<tr>
		<td colspan="2" style="font-size:13px;">
			position:absolute(表示绝对定位),这条语句的作用将元素从文档流中拖出来,然后使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位.如果不存在这样的包含块,则相对于body元素,即相对于浏览器窗口.
		</td>
	</tr>
	<tr>
		<td style="width:50%;">
			<pre>		
div{
    width:200px;
    height:200px;
    border:2px red solid;
    position:absolute;
    left:100px;
    top:50px;
}
			</pre>
		</td>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBf3378068267d3914e1a0d0e18a90a0cf?method=download&shareKey=89fb1be6d4f0e60c82017b53601bb9a4">
		</td>
	</tr>
</table>

* 相对定位(position: relative)

<table>
	<tr>
		<td colspan="2" style="font-size:13px;">
			position:relative(表示相对定位),它通过left、right、top、bottom属性确定元素在正常文档流中的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来),然后相对于以前的位置移动,移动的方向和幅度由left、right、top、bottom属性确定,偏移前的位置保留不动
		</td>
	</tr>
	<tr>
		<td style="width:50%;">
			<pre>		
div{
    width:200px;
    height:200px;
    border:2px red solid;
    position:relative;
    left:100px;
    top:50px;
}
			</pre>
		</td>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBa97139fd033f5c7d0347fffbad26910f?method=download&shareKey=2d87b3136656deaed9733a19f25d2433">
		</td>
	</tr>
</table>

* 固定定位(position: fixed)

<table>
	<tr>
		<td style="width:50%;" style="font-size:13px;">
			fixed：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是窗口。由于窗口本身是固定的，它不会随浏览器窗口的滚动条滚动而变化
		</td>
		<td style="width:50%;">
<pre>
	div{
		width: 200px;
		height: 200px;
		border: 2px red solid;
		position: fixed;
		left: 100px;
		top: 100px;
	}
</pre>
		</td>
	</tr>
</table>

#### 4.3 前端布局

##### 4.3.1 一列布局

```xml
<div style="width: 1200px;height: 500px;background-color: pink;margin: 0 auto;"></div>
```

##### 4.3.2 二列布局

```xml
<div style="width: 100%;height: 500px;background-color: pink;margin: 0 auto;">
	<div style="width: 50%;height: 500px;background-color: red;float: left;"></div>
	<div style="width: 50%;height: 500px;background-color: blue;float: left;"></div>
</div>
```

##### 4.3.3 三列布局

```xml
<div style="width: 100%;height: 500px;background-color: pink;margin: 0 auto;">
	<div style="width: 33.33%;height: 500px;background-color: red;float: left;"></div>
	<div style="width: 33.33%;height: 500px;background-color: blue;float: left;"></div>
	<div style="width: 33.33%;height: 500px;background-color: yellow;float: left;"></div>
</div>
```

## 第八章 CSS样式设置小技巧

### 第1节 水平居中设置(内联元素)

```xml
被设置元素为文本、图片等行内元素时，水平居中是通过给父元素设置 text-align:center 来实现的
```

### 第2节 水平居中设置(定宽块状元素)

```xml
定宽块状元素,设置自身为 margin-left：auto;margin-right:auto
```

### 第3节 水平居中设置(不定宽块状元素)

```xml
<style type="text/css">
	.container{
		text-align: center;
	}
	.container>ul{
		display: inline;
	}
	
</style>

<div class="container">
	<ul>
		<li><a href="#">1</a> </li>
		<li><a href="#">2</a> </li>
		<li><a href="#">3</a> </li>
	</ul>
</div>
```

### 第4节 垂直居中(父元素高度确定的单行文本)

```xml
父元素高度确定的单行文本的竖直居中的方法是通过设置父元素的height和line-height 高度一致来实现的

<div class="container">
	Hi 大呲花
</div>

<style type="text/css">
	.container{
		background-color: red;
		height: 50px;
		line-height: 50px;
	}
</style>
```

### 第5节 垂直居中(父元素高度确定的多行文本)

```xml
<div class="container">
	<div>
		<p>Hi 大呲花</p>
		<p>Hi 大呲花</p>
		<p>Hi 大呲花</p>
		<p>Hi 大呲花</p>
		<p>Hi 大呲花</p>
	</div>
</div>

.container{
	background-color: red;
	height: 500px;
	display: table-cell;
	vertical-align: middle;
}
```

## 第九章 Flex弹性盒模型

### 第1节 Flex介绍

#### 1.1 Flex布局是什么

```xml
Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性,任何一个容器都可以指定为 Flex 布局.

.box{
  display: flex;
}

Webkit 内核的浏览器，必须加上-webkit前缀(这是兼容老版本浏览器的写法)

.box{
  display: -webkit-flex; /* Safari,chrome */
  display: flex;
}

注意: 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效
```

#### 1.2 Flex基本概念

<table>
	<tr>
		<td colspan="2" style="font-size:13px;">
			采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
		</td>
	</tr>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBeea1915ed52f0a8f2b56ced3f6c9a354?method=download&shareKey=86a091daea346406f40559a6147fa9e4" >
		</td>
		<td style="width:50%;font-size:12px;">
			容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end
			<hr>
			项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size
		</td>
	</tr>
</table>

#### 1.3 容器的属性

```xml
以下6个属性设置在容器上。

flex-direction
flex-wrap
flex-flow
justify-content
align-items
align-content
```

### 第2节 容器的属性使用


* [1] flex-direction属性

```
flex-direction属性决定主轴的方向（即项目的排列方向）

.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEB9d7a12be7ae96b07f83f57fcd1f95001?method=download&shareKey=8bff76a0d788313212eb520ed093c400">
		</td>
		<td style="width:50%;">
			<pre>
它可能有4个值
* row（默认值）：主轴为水平方向，起点在左端。
* row-reverse：主轴为水平方向，起点在右端。
* column：主轴为垂直方向，起点在上沿。
* column-reverse：主轴为垂直方向，起点在下沿。
			</pre>
		</td>
	</tr>
</table>

* [2] flex-wrap属性

```
默认情况下，项目都排在一条线（又称”轴线”）上。flex-wrap属性定义，如果一条轴线排不下，如何换行.

.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBbc11f7db7f60f9dfcbefed37f8d3d063?method=download&shareKey=88730914f36fc9d19aebf8319f42f521">
		</td>
		<td>
			<pre>
它可能取三个值
1. nowrap（默认）：不换
2. wrap：换行，第一行在上方
3. wrap-reverse：换行，第一行在下方
			</pre>
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB237c84af94d87bef0fe25eefd55a18cf?method=download&shareKey=a1afbaec3f6be2ab2f6655a9a4f0c1f3">
		</td>
		<td>
			nowrap（默认）：不换
		</td>		
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB77ec43ac132771cb01e0c46627d96a60?method=download&shareKey=4040706db504611b866e9648330977a0">
		</td>
		<td>
			wrap：换行,第一行在上方
		</td>		
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB6575ac2ade467686c75159faf4eb04a4?method=download&shareKey=8360b2ca52282ef3188de831dd799450">
		</td>
		<td>
			wrap-reverse：换行，第一行在下方
		</td>		
	</tr>
</table>

* [3] flex-flow属性

```
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

.box {
  flex-flow: <flex-direction> <flex-wrap>;
}
```

* [4] justify-content属性

```
justify-content属性定义了项目在主轴上的对齐方式

.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```


<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEB58f5c97dbc113873b74569b541674324?method=download&shareKey=9fae04952163cae157f46b70a3c6fba6">
		</td>
		<td style="width:50%;font-size:12px;">
			<pre>
它可能取5个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右。
flex-start（默认值）：左对齐
flex-end：右对齐
center： 居中
space-between：两端对齐，项目之间的间隔都相等。
space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍
			</pre>
		</td>
	</tr>
</table>

* [5] align-items属性

```
align-items属性定义项目在交叉轴上如何对齐

.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEB1bdb428615c0b536483394815dd282f1?method=download&shareKey=d9a709193ffe4fae7ed8ba101781578a">
		</td>
		<td style="width:50%;font-size:12px;">
			<pre>
它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。
flex-start：交叉轴的起点对齐。
flex-end：交叉轴的终点对齐。
center：交叉轴的中点对齐。
baseline: 项目的第一行文字的基线对齐。
stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
			</pre>
		</td>
	</tr>
</table>

* [6] align-content属性

```
align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

<table>
	<tr>
		<td style="width:50%;">
			<img src="https://note.youdao.com/yws/api/personal/file/WEBfed36f15d8781404d6107fcec1b99242?method=download&shareKey=93b8adbf6c008e051b8dd008f892500f">
		</td>
		<td style="width:50%;font-size:12px;">
			<pre>
该属性可能取6个值。
flex-start：与交叉轴的起点对齐。
flex-end：与交叉轴的终点对齐。
center：与交叉轴的中点对齐。
space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
stretch（默认值）：轴线占满整个交叉轴
			</pre>
		</td>
	</tr>
</table>


## 第十章 其他常用前端属性介绍

```xml
1. 设置圆角
	border-radius: 3px;
	
2. 取消a标签的默认下划线
	text-decoration: none;

3. 取消无序列表的点
	list-style: none;

4. 取消input的边框以及焦点
	outline: none;
	border: none;

5. 设置背景图片
	background-image: url(img/timg.jpg);默认沿着水平方向和垂直方向进行平铺
	
	background-repeat: repeat-y; 沿着垂直方向
	background-repeat: repeat-x; 沿着水平方向
	background-repeat: no-repeat; 不平铺
```

## 第十一章 文本溢出处理

<img src="https://note.youdao.com/yws/api/personal/file/WEBc071f7b1a08d42c211d416f4547bac31?method=download&shareKey=bab32622f8c16c8f905c0d76b2d6feb3">

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>文本溢出</title>
		<style>
			.box{
				/* 
					当父容器的宽度不能装下当前文本时,文本会折行. 
					如果不想让文本折行,而是隐藏溢出文本并显示省略号.
					注意: 父容器宽度固定,可以是具体像素,也可以是百分数.
				*/
				width: 100px;/* 也可以是 width: 10%; */
				background-color: pink;
				/* 溢出不换行 */
				white-space: nowrap;
				/* 溢出隐藏 */
				overflow: hidden;
				/* 显示省略号 */
				text-overflow: ellipsis;
			}
			.box2{
				width: 200px;/* 也可以是 width: 20%; */
				background-color: pink;
				/* 超出部分隐藏 */
				overflow: hidden;
				/* 弹性伸缩盒子 */
				display: -webkit-box;
				/* 子元素垂直排列,里面的文本为子元素 */
				-webkit-box-orient: vertical;
				/* 文本超过三行显示省略号 */
				-webkit-line-clamp: 3;
			}
		</style>
	</head>
	<body>
		<!-- 单行文本溢出 -->
		<div class="box">
			这是一段文本,文本溢出了
		</div>
		<hr>
		<!-- 多行文本溢出 -->
		<div class="box2">
			这是一段多行文本,多行文本溢出,这里以显示三行为例,如果文本内容大于三行,那么三行以后显示省略号
		</div>
	</body>
</html>
```

## 第十二章 Google搜索首页练习

<table>
	<tr>
		<td style="width:60%;">
			<img src="https://note.youdao.com/yws/api/personal/file/45721472843543AA9C066193D8A44452?method=download&shareKey=f7cae99ac9158e4abe575667e330ffe1">
		</td>
		<td style="width:40%;font-size:10px;">
<code>https://www.xiaopiu.com/web/byId?type=project&id=5d6a109262ad1e4d5efc56f6</code>
		</td>
	</tr>
</table>