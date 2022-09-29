---
title: 02-HTML笔记
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

## 第一章 HTML介绍
```xml
HTML(HyperText Markup Language)超文本标记语言,是构成前端网页的基本单元.

它包含两层意思:
	1. HTML指它是一个以.html结尾的文件,是构成网站前端的基本单元
	2. HTML还指.html文件中的内容
```

<!-- more -->

## 第二章 HTML文件解析

<table>
	<tr>
		<td><img src="https://note.youdao.com/yws/api/personal/file/WEB3ba0a07ba17aed27b53fca0216b1e905?method=download&shareKey=6bb4969936391e750c02e3c9ee76818e"> </td>
	</tr>
</table>

```xml
网站的前端是由一个个的.html文件组成,我们在浏览器输入网站域名(网址)之后就会加载网站的前端页面(www.baidu.com),然后在浏览器中展示出来.

html文件从后端通过网络传递到前端依然是一个html文件,但是在浏览器中打开了之后却变成了漂亮的带有样式的内容,说明浏览器将html文件解析了。

所以前端开发需要浏览器作为编译和运行工具,这里推荐使用谷歌浏览器.
```

## 第三章 HTML文件的基本格式

```xml
程序要放在文件中,例如 java代码编写在.java文件中,C语言代码要放在.c文件中,前端html开发代码要放在一个.html文件中,文件中的内容格式如下:

<!DOCTYPE html>
<html>
	<head>
		
	</head>
	<body>
	
	</body>
</html>

HTML文件模板有一些固定格式
1. .html文件里的内容都是由一个个的标签构成,标签就是由左尖角号和右尖角号中间加上内容组成
	1.1 标签格式可以是左开右闭的标签,成对出现 例如: <html></html>
	1.2 标签格式也可以是自闭合标签,单个出现   例如: <hr>或者<hr/> 都可以
	
2. 固定模板介绍: 上面说.html文件内容是由一个个的标签组成,咱们详细介绍一下这个固定格式的前端模板
	2.1 <!DOCTYPE html>: 自闭合标签,放在html文件的首行,告诉浏览器在解析当前文件的时候,使用什么HTML版本进行解析.
		2.1.1 当前标签(<!DOCTYPE html>)意义是当前文件中的标签需要浏览器采用HTML5版本进行解析
		2.1.2 在HTML5版本之前还有HTML4版本,HTML4版本的标签为<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
		2.1.3 HTML5版本相比于HTML4版本添加了很多新特性,如果声明HTML4版本,很多HTML5版本的新特性就不能在当前html文件中使用
		2.1.4 软件升级一般都是向下兼容,也就是我们所说的高版本兼容低版本
		
	2.2 <html></html>: 左开右闭的标签,是html模板的基标签,除了首行的声明标签,整合html文件的标签都必须写在此标签之内.
	2.3 <head></head>: 左开右闭的标签,对当前页面的一些基础配置需要在这个标签中完成(比如当前html文件的标题,当前文件的编码,当前文件是否支持手机端浏览器[响应式]等)
	2.4 <body></body>: 左开右闭的标签,当前html文件在浏览器中显示出来的内容都必须放在此标签中,<head>标签的内容不会再浏览器中显示
```

## 第四章 HTML标签介绍

### 第1节 head中常见标签
#### 1.1 meta标签

```xml
<meta>:自闭合标签,可以配置当前网站的元数据信息,主要针对于搜索引擎以及字符编码,meta标签有4个重要属性 name、content、charset、http-equiv
	1. charset属性: 设置当前页面编码,例如:设置当前页面编码为utf-8  <meta charset="utf-8">
	2. name属性: name属性用于网页描述一般与content属性成对出现,name属性有多个值对应异同的content属性的值
		2.1 name属性值为keywords: keywords关键字,我们编写的网站可以被搜索引擎搜到关键字是其中非常重要的一个属性 例如: <meta name="keywords" content="汽车,汽车之家,汽车网"/>
		2.2 name属性值为description: 描述信息,使用搜索引擎搜索网站时会出现网站列表,列表中的网站都会有一段网站介绍,一般为描述的content值 例如: <meta name="description" content="汽车之家为您提供最新汽车报价"/>
	3. http-equiv属性: 可以向浏览器传递一些信息以帮助浏览器正确显示网页.http-equiv属性也有多个值对应content属性值
		3.1 http-equiv属性值为 content-type: 当前网页内容以及编码 例如: <meta http-equiv="content-type" content="text/html;charset=utf-8"/>
		3.2 http-equiv属性值为 refresh: 刷新页面 例如: 
			3.2.1 每隔2秒刷新一次 <meta http-equiv="refresh" content="2"/>
			3.2.2 10秒后跳转到另一个地址 <meta http-equiv="refresh" content="10;https://www.baidu.com"/>
		3.3 http-equiv属性值为 X-UA-Compatible: 如果不是IE浏览器此属性不会生效,如果是IE浏览器可以指定浏览器使用哪种引擎解析当前页面  例如: <meta http-equiv="X-UA-Compatible" content="IE=Edge;IE=IE11"/>
```

#### 1.2 title标签

```xml
<title>标签: 左开右闭标签,一个浏览器可以打开多个标签页,title属性设置标签名. 例如: <title>百度一下,你就知道</title>
```

#### 1.3 link标签

```xml
<link>标签: 自闭合标签,主要有两个功能
	1. 引入css文件,下面将css时候详细介绍
		1.1 引入css文件: <link rel="stylesheet" type="text/css" href="dist/css/bootstrap.css"/>
	2. 设置标签的图标,这个图标不会展示在浏览器内容区,而是展示在title标签中
		2.1 设置标签图标(网络图标): <link rel="icon" href="https://www.jd.com/favicon.ico"/>
		2.2 设置标签图标(本地图标): <link rel="icon" href="favicon.ico"/>
```

### 第2节 body中常见标签

#### 2.1 标题标签(h1~h6)

```xml
标题标签,如果在前端想展示文章标题可以使用此标签,标题一共分为6个级别
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```

#### 2.2 段落标签(p)

```xml
段落标签,一篇文章分为多段,一般用p标签

<p>段落文本</p>

注意:如果一篇文章有3个段落时，要放在3个p标签中
```
#### 2.3 简称或缩写标签(abbr)

```xml
<p>超文本传输协议写简称为<abbr>HTML</abbr></p>
如果一句话中有简称或者缩写(比如HTML是HyperText Markup Language的缩写),将缩写内容放在此标签中
```

#### 2.4 强调标签(strong/em)

```xml
em    :强调        -> 默认是斜体
strong:更强烈的强调 -> 粗体

<em>强调</em>
<strong>强烈的强调</strong>
```

#### 2.5 单行代码(code)

```xml
添加一行代码,一般计算机代码放在这个标签中

<code>一行代码</code>
```
#### 2.6 多行代码(pre)

```xml
添加一段代码

<pre>多行代码</pre>
```
#### 2.7 短引用标签(q)

```xml
短引用文本: 一句话如果引用一句经典语录,设置到此标签中

<q>短引用文本</q>
```
#### 2.8 长引用(blockquote)

```xml
长引用: 一句话如果引用多句经典语录,设置到此标签中

<blockquote> 长引用 </blockquote>
```
#### 2.9 换行标签(br)

```xml
换行标签

<h2>枫桥夜泊</h2>
<p>
   月落乌啼霜满天，<br>
   江枫渔火对愁眠。<br>
   姑苏城外寒山寺，<br>
   夜半钟声到客船。
</p>
```
#### 2.10 分割线标签(hr)

```xml
分割线

<p>火车飞驰过暗夜里的村庄,月光,总是太容易让思念寂寞,太容易让人觉得孤独</p>
<hr>    
<p>每一枚被风吹起的蒲公英,都载满了一双眼睛的深情告别与一个目光的依依不舍</p>
```
#### 2.11 无序列表标签(ul+li)

```xml
无序列表

<ul>
  <li>精彩少年</li>
  <li>美丽突然出现</li>
  <li>触动心灵的旋律</li>
</ul>
```
#### 2.12 有序列表标签(ol+li)

```xml
有序列表

<ol>
   <li>前端开发面试心法 </li>
   <li>零基础学习html</li>
   <li>JavaScript全攻略</li>
</ol>
```
#### 2.13 图片标签(img)

```xml
图片

<!-- 网络图片 -->
<img src="http://i0.hdslb.com/bfs/article/138225f66e58191efab2c2fdbbf95799aaad8c39.jpg">
<!-- 本地图片 -->
<img src="img/timg.jpg" alt="图片地址错误显示" title="鼠标移到图片上显示"/>
<!-- 云图片 -->
<img src="https://note.youdao.com/yws/api/personal/file/WEB6e9e40071bff499ec0fdae4756359ec5?method=download&shareKey=a722ddd923ca202724d9da80ca42b68a" />
```
#### 2.14 超链接标签(a)

```xml
超链接: 跳转另一个地址

<a href="目标地址">零基础学习Java</a>
	href属性: 点击a标签跳转地址
```
#### 2.15 表格标签(table)

```xml
表格: 生成表格

第一种表格:
<table border="1">
    <caption>员工通讯录</caption>
    <tr>
        <th>姓名</th>
        <th>性别</th>
        <th>职位</th>
    </tr>
    <tr>
        <td>张三</td>
        <td>男</td>
        <td>产品经理</td>
    </tr>
    <tr>
        <td>李四</td>
        <td>男</td>
        <td>开发工程师</td>
    </tr>
</table>

第二种表格:
<!-- 
    <thead><tbody><tfooter>标签作用: 如果不加表格会加载完成之后在显示,如果加了,表格tbody里的内容就会按需加载,防止数据过大,加载缓慢问题
    colspan: 水平合并单元格
 -->
<table border="1">
    <caption>员工通讯录</caption>
    <thead>
        <tr>
            <th>姓名</th>
            <th>性别</th>
            <th>职位</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>男</td>
            <td>产品经理</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>男</td>
            <td>开发工程师</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>合计</td>
            <td colspan="2">100</td>
        </tr>
    </tfoot>
</table>
```
#### 2.16 表单标签(form)

```xml
表单

<form>
    性别:
    <label for="male">男</label>
    <input type="radio" name="gender" checked id="male"/>
    <label for="female">女</label>
    <input type="radio" name="gender" id="female"/><br/>
    兴趣:
    篮球 <input type="checkbox" name="a" />
    足球 <input type="checkbox" checked name="b" />
    台球 <input type="checkbox" name="c" />
    <br/>
    <label for="email">邮箱</label>
    <input type="email" name="email" id="email" placeholder="输入邮箱地址"/><br/>
    <label for="desc">描述</label>
    <textarea value="文本域" name="desc" id="desc"></textarea><br/>
    <label for="address">地址</label>
    <select id="address">
        <option>北京</option>
        <option>上海</option>
        <option>天津</option>
        <option>南京</option>
    </select>
    <br/>
    <input type="submit" value="提交" />
    <input type="reset" value="重置" />
</form>
```
#### 2.17 按钮标签(button)

```xml
按钮标签: 生成按钮

<button type="button">我是一个按钮</button>
```
#### 2.18 包含标签(iframe)

```xml
iframe标签可以包含(或者引入)另一个页面到当前标签中

<iframe src="另一个html地址"></iframe>

HTML5版本中的属性介绍

frameborder: 0/1  是否显示边框  1:显示;0:不显示
scrolling: yes/no/auto 是否显示滚动条 
src: 显示另一个页面的页面地址
```

#### 2.19 其它标签

```xml
div和span标签,本身没什么意义,主要用来布局,后面学习CSS时候在详细介绍.
```

#### 2.20 特殊字符

```xml
1. 空格( ) : &nbsp;
2. 大于号(>) : &gt;
3. 小于号(<) : &lt;
4. 单个双引号(") : &quot;
5. 与号(&) : &amp;
```

#### 2.21 HTML5新增标签

```xml
1. article标签

	标签是 HTML 5 中的新标签,用于描述文章
	<article>
	  <h1>标题</h1>
	  <p>文章第一段</p>
	  <p>文章第二段</p>
	</article>

2. header标签
	
	标签是 HTML 5 中的新标签,标签定义文档的页眉(文档介绍信息)
	<header>
	    <h1>文档标题</h1>
	    <p>文档内容</p>
	</header>

3. footer标签
	
	标签是 HTML 5 中的新标签,标签定义文档的页脚
	<footer>
	  <p>网站: https://ukoko.gitee.io/</p>
	  <p>邮箱: <a href="#">hd1611756908@163.com</a></p>
	</footer>

4. section标签
	
	标签是 HTML 5 中的新标签,标签定义文档中的节.比如:章节、页眉、页脚或文档中的其他部分
	<section>
	  <p>第一节</p>
	</section>

5. audio/video标签
	
	<!-- 
	    音频标签
	    controls: 给音频添加播放控件,例如 开始,进度条,声音等
	    autoplay: 自动播放
	    loop: 循环播放
	    muted: 静默运行
	
	    source: 设置多个不同格式的音频地址防止浏览器不兼容,支持格式 mp3、ogg、wav ,不是所有浏览器都支持
	 -->
	 <audio controls autoplay loop muted>
	     <source src="xxx.mp3" type="audio/mpeg">
	 </audio>
	
	 <!-- 
	    视频标签
	    height: 高度
	    width: 宽度
	    controls: 给视频添加播放控件,例如 开始,进度条,声音等
	    autoplay: 自动播放
	    loop: 循环播放
	    muted: 静默运行
	    poster: 视频未运行之前的图片展示的图片地址
	
	    source: 设置多个不同格式的视频地址防止浏览器不兼容,支持格式 mp4、ogg、webm ,不是所有浏览器都支持
	 -->
	 <video height="500" width="500" controls autoplay loop muted poster="tiger.gif">
	     <source src="wzry.mp4" type="video/mp4"></source>
	 </video>

注意: HTML标签有很多这里只是介绍了一些比较常见的标签,如果工作中需要其他标签,可以查询文档.
```

## 第五章 HTML标签语义化
### 第1节 什么是语义化

```xml
在浏览器中显示出来的每一个内容赋予其特殊的意义
```

### 第2节 HTML为什么设计那么多标签

```xml
浏览器在解析HTML文件时,对于用户来说div标签和p标签解析出来的样式没有任何区别的,那为什么还要设置那么多标签呢?为什么不使用一种标签?

因为这些标签本身不是给我们用户看的,是给浏览器(SEO搜索引擎)看的,浏览器根据不同的标签手机关键字,收录到自己的网站中,不同的标签代表不同的寓意,关键字的重要性不相同.
```
### 第3节 语义化的好处

```xml
1. 更容易被搜索引擎收录
2. 更容易让屏幕阅读器读出网页内容
```

---

<div style="font-size:10px;text-align:center;"><strong style="color:blue;">※※※※※※</strong> 我的梦想是有很多很多钱,有钱可以实现很多梦想.如果上一个梦想不能实现,那么我想有点钱,有点够花的钱(~(@^_^@)~) <strong style="color:blue;">※※※※※※</strong></div>

---
