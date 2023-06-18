---
title: Layui笔记
categories: 前端
tags: ["前端","html","css","js","javascript","layui"]
---

```xml
作者 : 夜泊1990
邮箱 : hd1611756908@163.com
企鹅 : 1611756908
鹅群 : 948233848
博客 : https://hd1611756908.github.io/
破站 : https://space.bilibili.com/514155929/
```

## 第一章 Layui是什么
```
Layui 是一套免费的开源 Web UI 组件库,遵循原生态的 HTML/CSS/JavaScript 开发模式,极易上手,拿来即用


学习框架的意义: 写更少的代码,实现相同的功能.

```

<!-- more -->

## 第二章 Layui环境搭建

### 第1节 官网地址

```
https://layui.dev/
```

### 第2节 环境搭建

```
方式一 : 在线第三方 CDN 引入,UNPKG 和 CDNJS 均为第三方免费 CDN (详细请看官网)

	<!-- 引入 layui.css -->
	<link href="//unpkg.com/layui@2.8.7/dist/css/layui.css" rel="stylesheet">
	<!-- 引入 layui.js -->
	<script src="//unpkg.com/layui@2.8.7/dist/layui.js">

方式二 : 离线引入

	离线引入方式需要去官网下载layui安装包,将css文件和js文件复制粘贴到项目中,然后在引入
	下载地址: https://foruda.gitee.com/attach_file/1686881337969335393/layui-v2.8.7.zip?token=d4d056410abf7bddebc92f57eae6d7b3&ts=1687053097&attname=layui-v2.8.7.zip

方式三 : npm 引入(略)
```

```html
<!-- 前端模板如下: -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>layui</title>
  <!-- 引入 layui.css -->
  <link href="//unpkg.com/layui@2.8.7/dist/css/layui.css" rel="stylesheet">
</head>
<body>
 
<!-- 你的 HTML 代码 -->

<!-- 引入 layui.js -->
<script src="//unpkg.com/layui@2.8.7/dist/layui.js">
</body>
</html>
```


## 第三章 Layui知识点大纲

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB21ac8d4b37b5ac66d29aa5cb990c4485?method=download&shareKey=d5a3151826352a7df34e5fd0670b7b14">
		</td>
	</tr>
	<tr>
		<td style="font-size:14px;">
			大纲中只列举了常用的Layui前端知识点,其它的请查看官网.
		</td>
	</tr>
</table>

## 第四章 Layui页面元素

### 第1节 布局元素
#### 1.1 栅格布局

```
https://layui.dev/2.7/docs/element/layout.html#grid
```

* 案例展示

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBe50abf1265b5579f1c23baded13292c1?method=download&shareKey=c526d658c61d5032a2b6ae8ffb2642ed">
		</td>
	</tr>
</table>

* 传统布局(略)

```
使用传统布局实现案例样式代码量会比较多
```

* Layui布局

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Layui布局</title>
		<!-- 引入 layui.css -->
		<link href="//unpkg.com/layui@2.8.7/dist/css/layui.css" rel="stylesheet">
	</head>
	<body>
		<!-- 布局容器,水平居中  -->
		<div class="layui-container">
		  <div class="layui-row">
			  <div class="layui-col-md3" style="height: 350px;border: 1px solid #ccc;"></div>
			  <div class="layui-col-md9" style="height: 350px;border: 1px solid royalblue;">
				  <div class="layui-row" style="height: 45px;border: 1px solid red;"></div>
				  <div class="layui-row">
				  	<div class="layui-col-md9" style="height: 303px;border: 1px solid fuchsia;"></div>
					<div class="layui-col-md3"></div>
				  </div>
			  </div>
		  </div>
		</div>
		<!-- 引入 layui.js -->
		<script src="//unpkg.com/layui@2.8.7/dist/layui.js
	</body>
</html>
```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB9add7cbd09926e03f49d752977b54018?method=download&shareKey=c486215f6d476b6415e003389960f70f">
		</td>
	</tr>
</table>

#### 1.2 布局容器

```
https://layui.dev/2.7/docs/element/layout.html#grid-container
```
```
/* 宽度固定水平居中 */
<div class="layui-container"></div>
/* 宽度100% */
<div class="layui-fluid"></div>
```
### 第2节 表格

```
https://layui.dev/2.7/docs/element/table.html
```

### 第3节 按钮/图标

```
https://layui.dev/2.7/docs/element/button.html
https://layui.dev/2.7/docs/element/icon.html
```

### 第4节 表单

```
https://layui.dev/2.7/docs/element/form.html
```


### 第5节 其它

```
动画、导航、菜单等...
```

## 第五章 Layui内置模块
### 第1节 分页模块

```
https://layui.dev/2.7/docs/modules/laypage.html
```

### 第2节 数据表格
```
https://layui.dev/2.7/docs/modules/table.html
```
### 第3节 弹出层
```
https://layui.dev/2.7/docs/modules/layer.html
```
### 第4节 表单模块
```
https://layui.dev/2.7/docs/modules/form.html
```
### 第5节 时间模块
```
https://layui.dev/2.7/docs/modules/laydate.html
```
### 第6节 文件上传模块
```
https://layui.dev/2.7/docs/modules/upload.html
```
### 第7节 树状表格模块
```
方式 1 : layui2.7版本不带树状表格,需要借助第三方   https://dev.layuion.com/extend/
方式 2 : layui2.8版本新增树状表格,使用起来更方便   https://layui.dev/docs/2.8/treeTable/
```

### 第8节 其它模块
```
以上介绍的模块是开发中常见的模块,如果想使用其它模块,请查看官网
```

## 第六章 Layui2.8新特性

```
以上文档提示,没有特殊说明都是基于layui2.7版本的官网进行参考,2.8官网的使用上没有2.7方便,如果使用2.8新特性,在参考2.8官网.

layui2.8.x版本新增了较多的新特性,修复了很多的BUG,如果想了解新特性,请求考日志: https://layui.dev/docs/2.8/versions.html#2.8.0
```


## 第七章 Layui框架案例练习

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB2eadfa4f6e0f87168688ea8683588473?method=download&shareKey=7015e1c98c020325643b34e63bd2e2ae">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB741e218454ca65df495c29652aeab45f?method=download&shareKey=86a0feef51bccba151bba2c7412b799f">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB5c121c02596569089179271308ece9a7?method=download&shareKey=576a3cef81af88b71d7107a827e8a82c">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB4bc1666d3687c6d0e3948ef723e30721?method=download&shareKey=a9ce373bd180c681c0648220cf4601db">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBe920d2df0bcdcbc906456cc22e528d31?method=download&shareKey=25fdc7b659812ddd356129c7c7011702">
		</td>
	</tr>
</table>

## 第八章 Layui框架模板使用(Pear Admin Layui)
### 第1节 官网地址
```
http://www.pearadmin.com/
```

### 第2节 在线演示地址

```
http://layui.pearadmin.com/
```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBe44697216a8e98081787710d533306ae?method=download&shareKey=740fbc8a4d9805abbbf8616c0bad0ca6">
		</td>
	</tr>
</table>

### 第2节 CRUD练习(略)

---

<div style="font-size:10px;text-align:center;"><strong style="color:blue;">※※※※※※</strong> 大道至简(~(@^_^@)~) <strong style="color:blue;">※※※※※※</strong></div>

---