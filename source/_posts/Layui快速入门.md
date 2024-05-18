---
title: Layui快速入门
categories: Layui
tags: ["html","css","JavaScript","Layui","pearAdmin","JS"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://hd1611756908.github.io/
B 站: https://space.bilibili.com/514155929/
```

## 第一章 Layui是什么
```xml
Layui 是一套免费的开源 Web UI 组件库,遵循原生态的 HTML/CSS/JavaScript 开发模式,极易上手,拿来即用

学习框架的意义: 写更少的代码,实现相同的功能.
```

<!-- more -->

## 第二章 Layui环境搭建

### 第1节 官网地址

```
https://layui.dev/

当前版本 2.9.x 版本,相比于2.8.x和2.7.x新增了很多实用功能,下面会介绍.
```

### 第2节 环境搭建

```
方式一 : 在线第三方 CDN 引入,UNPKG 和 CDNJS 均为第三方免费 CDN (详细请看官网)

    <!-- 引入 layui.css -->
    <link href="//unpkg.com/layui@2.9.7/dist/css/layui.css" rel="stylesheet">
    <!-- 引入 layui.js -->
    <script src="//unpkg.com/layui@2.9.7/dist/layui.js"></script>

方式二 : 离线引入

	离线引入方式需要去官网下载layui安装包,将css文件和js文件复制粘贴到项目中,然后在引入
	下载地址: https://foruda.gitee.com/attach_file/1709083131303683727/layui-v2.9.7.zip?token=4d86625f99f8aa72a6131358c786aeb4&ts=1709353417&attname=layui-v2.9.7.zip

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
  <link href="//unpkg.com/layui@2.9.7/dist/css/layui.css" rel="stylesheet">
</head>
<body>
 
<!-- 你的 HTML 代码 -->

<!-- 引入 layui.js -->
<script src="//unpkg.com/layui@2.9.7/dist/layui.js"></script>
</body>
</html>
```


## 第三章 Layui知识点大纲

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBa5414dbd90479d79d9a5a9539ee12e0d?method=download&shareKey=4b8d238ab4f7e8c12ad2310701d2c067">
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
https://layui.dev/docs/2/layout/grid.html
```

* 案例展示

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBec329b24422feb6d79299856f591445d?method=download&shareKey=50201b0b98fed97eaf5b30be3509dfbc">
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
		<link href="//unpkg.com/layui@2.9.7/dist/css/layui.css" rel="stylesheet">
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
		<script src="//unpkg.com/layui@2.9.7/dist/layui.js
	</body>
</html>
```

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB523a50d186bcf3f3f1e30e4106e209a1?method=download&shareKey=484d05317dd2b41e30bc9ee3439eb558">
		</td>
	</tr>
</table>


#### 1.2 布局容器

```
https://layui.dev/docs/2/layout/grid.html#container
```
```
/* 宽度固定水平居中 */
<div class="layui-container"></div>
/* 宽度100% */
<div class="layui-fluid"></div>
```
### 第2节 表格

```
https://layui.dev/docs/2/table/
```

### 第3节 按钮/图标

```
https://layui.dev/docs/2/button/
https://layui.dev/docs/2/icon/
```

### 第4节 表单

```
https://layui.dev/docs/2/form/
```


### 第5节 其它

```
动画、导航、菜单等...
```

## 第五章 Layui内置模块
### 第1节 分页模块

```
https://layui.dev/docs/2/laypage/
```

### 第2节 数据表格
```
https://layui.dev/docs/2/table/
```
### 第3节 弹出层
```
https://layui.dev/docs/2/layer/
```
### 第4节 表单模块
```
https://layui.dev/docs/2/form/
```
### 第5节 时间模块
```
https://layui.dev/docs/2/laydate/
```
### 第6节 文件上传模块
```
https://layui.dev/docs/2/upload/
```
### 第7节 树状表格模块
```
方式 1 : layui2.7版本不带树状表格,需要借助第三方   https://dev.layuion.com/extend/
方式 2 : layui2.8/9版本新增树状表格,使用起来更方便   https://layui.dev/docs/2/treeTable/
```

### 第8节 其它模块
```
以上介绍的模块是开发中常见的模块,如果想使用其它模块,请查看官网
```

## 第六章 Layui框架案例练习

### 第1节 SQL语句

```sql

-- 创建数据库
CREATE DATABASE book DEFAULT CHARACTER SET UTF8;

-- 创建数据库表
CREATE TABLE category(
    category_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '分类ID',
    category_name VARCHAR(255) NOT NULL COMMENT '分类名称'
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '图书分类表';

-- 插入数据
INSERT INTO category(category_name) VALUES('科技'),('文学'),('历史'),('财经');

-- 创建图书表
CREATE TABLE book(
    book_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '图书ID',
    book_name VARCHAR(255) NOT NULL COMMENT '图书名称',
    author_name VARCHAR(255) NOT NULL COMMENT '图书作者',
    price DOUBLE(12,2) DEFAULT 0.00 COMMENT '图书价格',
    category_id INT NOT NULL COMMENT '分类ID,关联分类表主键',
    create_time DATETIME DEFAULT NULL COMMENT '创建时间',
    status INT DEFAULT 1 COMMENT '是否上架 1:上架 0:下架',
    book_url VARCHAR(255) DEFAULT NULL COMMENT '图书主图地址',
    book_address VARCHAR(255) DEFAULT NULL COMMENT '真实地址'
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '图书表';

-- 关联
ALTER TABLE book ADD FOREIGN KEY (category_id) REFERENCES category(category_id);
```

### 第2节 页面展示

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBcaa39e034539b0436c60175c4bc15f87?method=download&shareKey=5251378f90ebd97962429356230647af">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBfeb4a5ed6e47f0cff0af91fa4e2223c1?method=download&shareKey=80b34117c9de27640bd38f3fc42ab33c">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB1277a1620a1118934e164d8013075215?method=download&shareKey=7899d9be3b1c64ff16c4543a37f67577">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBbee41cf043b4124b0f2ff952d80420df?method=download&shareKey=beb1c52915054fdd7d901edd3e22b3f9">
		</td>
	</tr>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEBaed2d0b4ae0699e1456d033316e79bf9?method=download&shareKey=40a5537004c56713eca6ff7d5d5cc6bd">
		</td>
	</tr>
</table>


## 第七章 Layui框架模板使用(Pear Admin Layui)
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
			<img src="https://note.youdao.com/yws/api/personal/file/WEB1fc5dd0f446c590b847196457a44446e?method=download&shareKey=5e20f7c11b7c1038647a4a906598c93b">
		</td>
	</tr>
</table>


### 第2节 CRUD练习

#### 2.1 数据库表创建

```sql
-- 创建测试库
CREATE DATABASE auth DEFAULT CHARACTER SET UTF8;

-- 常见角色表
CREATE TABLE role(
    role_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '角色ID',
    role_name VARCHAR(255) NOT NULL COMMENT '角色名称',
    status TINYINT(1) NOT NULL DEFAULT 1 COMMENT '状态(定义逻辑删除) 1:有效 0:无效',
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '角色表';

-- 创建用户表
CREATE TABLE user(
    user_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
    user_name VARCHAR(255) NOT NULL COMMENT '用户名称',
    email VARCHAR(255) NOT NULL COMMENT '邮箱',
    password VARCHAR(255) NOT NULL COMMENT '密码',
    status TINYINT(1) NOT NULL DEFAULT 1 COMMENT '状态(定义逻辑删除) 1:有效 0:无效',
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '用户表';

-- 创建用户角色关联表
CREATE TABLE user_role(
    role_id INT NOT NULL  COMMENT '角色ID',
    user_id INT NOT NULL COMMENT '用户ID',
    status TINYINT(1) NOT NULL DEFAULT 1 COMMENT '状态(定义逻辑删除) 1:有效 0:无效',
    PRIMARY KEY(role_id,user_id)
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '用户角色关联表';

-- 外键关联
ALTER TABLE user_role ADD FOREIGN KEY (role_id) REFERENCES role(role_id);
ALTER TABLE user_role ADD FOREIGN KEY (user_id) REFERENCES user(user_id);

-- 菜单表
CREATE TABLE menu(
    id INT PRIMARY KEY COMMENT '菜单ID',
    title VARCHAR(255) NOT NULL COMMENT '菜单标题',
    icon  VARCHAR(255) COMMENT '菜单图标',
    type INT DEFAULT 0 COMMENT '菜单类型',
    openType VARCHAR(255) DEFAULT "" COMMENT "菜单类型名称",
    href VARCHAR(255) COMMENT "菜单指向地址",
    parent_id INT DEFAULT 0 COMMENT "菜单父ID,关联主键",
    level INT DEFAULT 1 COMMENT "菜单级别",
    status TINYINT DEFAULT 1 COMMENT "菜单状态 1:有效 0:禁用",
    update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '设置当前时间，并且自动更更新时间'
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '用户角色关联表';

-- 菜单角色关联表
CREATE TABLE menu_role(
    role_id INT NOT NULL  COMMENT '角色ID',
    id INT NOT NULL COMMENT '菜单ID',
    status TINYINT(1) NOT NULL DEFAULT 1 COMMENT '状态(定义逻辑删除) 1:有效 0:无效',
    PRIMARY KEY(role_id,id)
) DEFAULT CHARSET='UTF8' AUTO_INCREMENT=10000 COMMENT '菜单角色关联表';

-- 外键关联
ALTER TABLE menu_role ADD FOREIGN KEY (role_id) REFERENCES role(role_id);
ALTER TABLE menu_role ADD FOREIGN KEY (id) REFERENCES menu(id);
```

#### 2.2 页面展示

