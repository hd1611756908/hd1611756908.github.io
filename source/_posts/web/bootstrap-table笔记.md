---
title: bootstrap-table笔记
categories: 前端
tags: ["html","css","javascript","bootstrap","bootstrap-table","jquery"]
---

```xml
作者 : 夜泊1990
邮箱 : hd1611756908@163.com
企鹅 : 1611756908
鹅群 : 948233848
博客 : https://hd1611756908.github.io/
破站 : https://space.bilibili.com/514155929/
```

## 第一章 bootstrap-table概述

### 第1节 bootstrap-table介绍

```xml
Bootstrap Table 是一个最广泛的前端表格数据生成框架(支持Bootstrap, Semantic UI, Bulma, Material Design, Foundation)

官网地址: https://bootstrap-table.com/
```

<!-- more -->

### 第2节 bootstrap-table环境搭建

```
方式一: 离线引入
	1. 下载 https://github.com/wenzhixin/bootstrap-table/archive/master.zip
	2. 引入
	
方式二: 在线引入
	直接引入:
		
		<!-- 引入 bootstrap-table 的css样式库 -->
		<link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.21.0/dist/bootstrap-table.min.css">
		
		<!-- 引入 bootstrap-table 的 JS 库 -->
		<script src="https://unpkg.com/bootstrap-table@1.21.0/dist/bootstrap-table.min.js"></script>
		<!-- 引入 bootstrap-table 的 语言 库 -->
		<script src="https://unpkg.com/bootstrap-table@1.21.0/dist/locale/bootstrap-table-zh-CN.min.js"></script>
		
方式三: 包管理(略)
	使用npm包管理工具,命令如下: npm install bootstrap-table


注意事项: 在使用bootstrap-table时需要引入bootstrap环境文件
```

## 第二章 表格的动态生成

### 第0节 后端数据格式(样例)

```json
[
  {
    "roleId": 1,
    "roleName": "平台管理员",
    "isDelete": 1
  },
  {
    "roleId": 2,
    "roleName": "库管",
    "isDelete": 1
  },
  {
    "roleId": 3,
    "roleName": "普通角色0",
    "isDelete": 1
  }
]
```

### 第1节 标签属性方式生成表格

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>数据表格生成</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			bootstrap-table生成数据表格 方式一: 标签方式
			data-toggle="table" : 设置此属性的功能是将现有的未格式化的HTML转换成一个表(table)
			data-url="http://localhost:8080/getRoleAll" : 请求数据的地址
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleAll"
		>
		  <thead>
		    <tr>
			  <!--  
				data-field="roleId": json数据格式中对象对应的属性名称
			   -->
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
	</body>
</html>
```

```bat
官网地址: https://examples.bootstrap-table.com/#welcomes/from-url.html
```

### 第2节 AJAX方式生成表格

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>数据表格生成</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<table id="table">
		  <thead>
		    <tr>
		      <!--  
		    	data-field="roleId": json数据格式中对象对应的属性名称
		       -->
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			//获取表格对象
			var $table = $('#table');
			//通过AJAX从后台获取数据
			$.getJSON("http://localhost:8080/getRoleAll",function(data){
				//调用bootstrap-table中的bootstrapTable方法.
				$table.bootstrapTable({data: data});
			})
		</script>
	</body>
</html>
```

```bat
官网地址: https://examples.bootstrap-table.com/#welcomes/from-data.html
```

### 第3节 JS方式生成数据表格
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>数据表格生成</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 表格生成位置 -->
		<table 
			id="table"
			data-url="http://localhost:8080/getRoleAll"
		></table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			//1. 获取table的JQuery对象
			var $table=$("#table");
			//2. 调用bootstrapTable函数
			$table.bootstrapTable({
				// 自定义表的列
				columns:[
					{
						// 自定义列名称
						title:'角色ID',
						// 后台JSON数据中对应的属性名
						field:'roleId'
					},
					{
						title:'角色名称',
						field:'roleName'
					},
					{
						title:'是否删除',
						field:'isDelete'
					}
				]
			});
		</script>
	</body>
</html>

```
```bat
官网地址: https://examples.bootstrap-table.com/#options/table-columns.html
```
### 第4节 文档API详解

#### 4.1 API地址
```
https://bootstrap-table.com/docs/api/table-options/
```
#### 4.2 API属性详解

<table>
	<tr>
		<td>
			<img src="https://note.youdao.com/yws/api/personal/file/WEB5e7c1b1094892adc66e65b9134180097?method=download&shareKey=13bc8ad708cbeef9a42d8bfb1ec5e231">
		</td>
	</tr>
</table>

#### 4.3 举例介绍
```html
在 第3节 JS方式生成数据表格中,数据获取用的是table标签中的data-url属性,可以去掉这个属性,使用url属性获取,这时需要将url属性设置在bootstrapTable函数中.

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>数据表格生成</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 表格生成位置 -->
		<table id="table"></table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			//1. 获取table的JQuery对象
			var $table=$("#table");
			//2. 调用bootstrapTable函数
			$table.bootstrapTable({
				//数据使用url属性获取,代替表格的data-url属性
				url:"http://localhost:8080/getRoleAll",
				// 自定义表的列
				columns:[
					{
						// 自定义列名称
						title:'角色ID',
						// 后台JSON数据中对应的属性名
						field:'roleId'
					},
					{
						title:'角色名称',
						field:'roleName'
					},
					{
						title:'是否删除',
						field:'isDelete'
					}
				]
			});
		</script>
	</body>
</html>
```

## 第四章 开启分页

### 第0节 官网默认分页数据格式

```json
/* total: 总记录数 <--> rows : 当前页数据 */
{ 
  "total": 800,
  "rows": [
    {
      "id": 0,
      "name": "Item 0",
      "price": "$0"
    },
    {
      "id": 1,
      "name": "Item 1",
      "price": "$1"
    },
    {
      "id": 2,
      "name": "Item 2",
      "price": "$2"
    },
    {
      "id": 3,
      "name": "Item 3",
      "price": "$3"
    }
  ]
}
```

### 第1节 分页开启以及定制化

#### 1.1 开启分页方式一(标签开启)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			data-pagination="true" : 开启分页
			data-side-pagination="server" :  
				1. 从服务器端获取数据(确定服务器端数据格式)
				2. 分页server的默认请求参数是 offset:偏移量 limit: 每页显示多少条数
				3. 不设置分页不会生效
			
			官网地址: 
				1. https://bootstrap-table.com/docs/api/table-options/#pagination
				2. https://bootstrap-table.com/docs/api/table-options/#sidepagination
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleBootstrap"
		  data-pagination="true"
		  data-side-pagination="server"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
	</body>
</html>
```
#### 1.2 开启分页方式二(JS函数开启)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/getRoleBootstrap",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据,如果不设置分页不会生效 */
				sidePagination:"server",
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```

#### 1.3 定制化分页一(标签方式)

##### 1.3.1 分页样式设置

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页样式</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			data-pagination-pre-text="上一页" : 上一页设置,默认值为 <
			data-pagination-next-text="下一页" : 下一页设置,默认值为 >
			
			官网地址: 
				1. https://bootstrap-table.com/docs/api/table-options/#paginationpretext
				2. https://bootstrap-table.com/docs/api/table-options/#paginationnexttext
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleBootstrap"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
	</body>
</html>
```

##### 1.3.2 分页参数设置

* 默认请求参数

```html
bootstrap-table分页数据默认分页请求参数为 limit(每页显示多少条数)和offset(偏移量)
如果想修改成 pageNo(当前页)和pageSize(每页显示多少条数)bootstrap-table 也是可以的.

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页样式</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			data-query-params-type="" : 设置分页的请求参数 默认值是 limit
				1. 如果使用默认值limit 那么它的默认请求参数为 limit, offset, search, sort, order
				2. 如果设置为空字符串(非默认值) 那么它的请求参数为 pageSize, pageNumber, searchText, sortName, sortOrder
			
			官网地址:
				1. https://bootstrap-table.com/docs/api/table-options/#queryparamstype
				2. https://bootstrap-table.com/docs/api/table-options/#queryparams
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRolePage"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params-type=""
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
	</body>
</html>
```

* 新增请求参数

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页参数</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			bootstrap-table分页查询默认自带请求参数,如果后端需要的请求参数和前端自带的不匹配,这是需要新增请求参数
			例如后端需要除了需要默认的分页请求参数外需要新增自定义参数 isDelete,添加方式如下
			data-query-params="queryParams": 这个属性是处理请求参数的,类型为一个函数,需要自定义个queryParams函数处理请求参数
			
			官网地址: https://bootstrap-table.com/docs/api/table-options/#queryparams
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleList"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params="queryParams"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			
			/* 
				处理请求参数
			 */
			function queryParams(params){
				console.log("--->>>",params);
				//设置isDelete请求参数
				params.isDelete=1;
				return params; //返回值将新的参数返回给bootstrap-table
			}
			
		</script>
	</body>
</html>
```

#### 1.4 定制化分页二(JS方式)

##### 1.4.1 分页样式设置

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页样式</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
			/* 
				官网地址:
					1. https://bootstrap-table.com/docs/api/table-options/#paginationpretext
					2. https://bootstrap-table.com/docs/api/table-options/#paginationnexttext
			 */
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/getRoleBootstrap",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```

##### 1.4.2 分页参数设置

* 默认请求参数

```html

bootstrap-table分页数据默认分页请求参数为 limit(每页显示多少条数)和offset(偏移量)
如果想修改成 pageNo(当前页)和pageSize(每页显示多少条数)bootstrap-table 也是可以的.

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页样式</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
			/* 
				官网地址:
					1. https://bootstrap-table.com/docs/api/table-options/#queryparamstype
					2. https://bootstrap-table.com/docs/api/table-options/#queryparams
			 */
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/getRolePage",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
				/* 
					设置分页的请求参数 默认值是 limit 
						1. 如果使用默认值limit 那么它的默认请求参数为 limit, offset, search, sort, order
						2. 如果设置为空字符串(非默认值) 那么它的请求参数为 pageSize, pageNumber, searchText, sortName, sortOrder
				*/
				queryParamsType:"",
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```

* 新增请求参数

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页参数</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
			/* 
				官网地址: https://bootstrap-table.com/docs/api/table-options/#queryparams
			 */
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/getRoleList",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
				/* 
					自定义参数设置 
				*/
				queryParams:function(params){
					params.isDelete=1;//自定义IsDelete参数
					return params;
				},
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```

#### 1.5 分页参数的其它配置

```html
1. 上一页下一页的循环模式配置

官网地址: https://bootstrap-table.com/docs/api/table-options/#paginationloop

2. 自定义每页显示多少条数的选择

官网地址: https://bootstrap-table.com/docs/api/table-options/#pagelist

3. 初始化当前页码以及页容量配置

官网地址:
	1. https://bootstrap-table.com/docs/api/table-options/#pagenumber
	2. https://bootstrap-table.com/docs/api/table-options/#pagesize


<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页其它配置</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			1. 默认首页从第几页开始 data-page-number="1": 默认值是1
			2. 每页显示多少条数    data-page-size="5"  : 默认值是10 
			3. 自定义选择每页显示多少条数 data-page-list="[5,10,15,20]" : 动态选择每页显示多少条数
			4. 分页循环模式(第一页时在点击上一页,跳转最后一个,反之亦然) data-pagination-loop="false": 默认值true
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleList"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params="queryParams"
		  data-page-number="1"
		  data-page-size="5"
		  data-page-list="[5,10,15,20]"
		  data-pagination-loop="false"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			
			/* 
				处理请求参数
			 */
			function queryParams(params){
				console.log("--->>>",params);
				//设置isDelete请求参数
				params.isDelete=1;
				return params; //返回值将新的参数返回给bootstrap-table
			}
			
		</script>
	</body>
</html>
```

## 第五章 响应数据处理

### 第0节 默认响应数据格式

```json
/* total: 总记录数 <--> rows : 当前页数据 这是bootstrap-table需要的默认数据格式*/
{ 
  "total": 800,
  "rows": [
    {
      "id": 0,
      "name": "Item 0",
      "price": "$0"
    },
    {
      "id": 1,
      "name": "Item 1",
      "price": "$1"
    },
    {
      "id": 2,
      "name": "Item 2",
      "price": "$2"
    },
    {
      "id": 3,
      "name": "Item 3",
      "price": "$3"
    }
  ]
}
/* 例如我们服务器端返回给前端的数据格式 */
{
  "code": 0,
  "msg": "OK",
  "obj": {
    "pageNo": 0,
    "pageSize": 0,
    "hasPre": false,
    "hasNext": true,
    "count": 102,
    "pageCount": 51,
    "data": [
      {
        "roleId": 1,
        "roleName": "平台管理员",
        "isDelete": 1
      },
      {
        "roleId": 2,
        "roleName": "库管",
        "isDelete": 1
      }
    ]
  }
}
// 大多时候服务器端返回给前端的数据可能不是bootstrap-table想要的,这时候就需要响应结果的重新处理.处理方式如下:
```
### 第1节 响应数据处理(标签)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>分页结果集处理</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			data-response-handler="responseHandler" : 结果集处理函数
			官网地址: https://bootstrap-table.com/docs/api/table-options/#responsehandler
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getRoleList"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params="queryParams"
		  data-response-handler="responseHandler"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			/* 
				处理请求参数
			 */
			function queryParams(params){
				//设置isDelete请求参数
				params.isDelete=1;
				return params; //返回值将新的参数返回给bootstrap-table
			}
			/*
				处理结果集
			*/ 
		   function responseHandler(res){
			   console.log(res.obj);
			   /*
				  重新定义结果集,将不符合bootstrap-table的结果集修改成满足bootstrap-table的数据结构
			   */
			  var d={};
			  d.total=res.obj.count;
			  d.rows=res.obj.data;
			  return d;
		   }
		</script>
	</body>
</html>
```
### 第2节 响应数据处理(JS)
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页结果集处理</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/getRoleList",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
				/* 
					自定义参数设置 
				*/
				queryParams:function(params){
					params.isDelete=1;//自定义IsDelete参数
					return params;
				},
				/*
					处理结果集,对结果集进行重新封装
					官网地址: https://bootstrap-table.com/docs/api/table-options/#responsehandler
				*/ 
			   responseHandler:function(res){
				   return {
					   total:res.obj.count,
					   rows:res.obj.data
				   }
			   },
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```
## 第六章 表格之搜索开启

### 第1节 开启搜索(标签)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>搜索开启</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 
			data-search="true" : 开启搜索 默认值为false,默认添加参数名为search
			官网地址: https://bootstrap-table.com/docs/api/table-options/#search
		 -->
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/searchRolePage"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params="queryParams"
		  data-response-handler="responseHandler"
		  data-search="true"
		>
		  <thead>
		    <tr>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			/* 
				处理请求参数
			 */
			function queryParams(params){
				//设置isDelete请求参数
				params.isDelete=1;
				return params; //返回值将新的参数返回给bootstrap-table
			}
			/*
				处理结果集
			*/ 
		   function responseHandler(res){
			   console.log(res.obj);
			   /*
				  重新定义结果集,将不符合bootstrap-table的结果集修改成满足bootstrap-table的数据结构
			   */
			  var d={};
			  d.total=res.obj.count;
			  d.rows=res.obj.data;
			  return d;
		   }
		</script>
	</body>
</html>
```

### 第2节 开启搜索(JS)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>搜索开启</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            //1. 获取table的JQuery对象
            var $table=$("#table");
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/searchRolePage",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
				/* 
					自定义参数设置 
				*/
				queryParams:function(params){
					params.isDelete=1;//自定义IsDelete参数
					return params;
				},
				/*
					处理结果集,对结果集进行重新封装
					官网地址: https://bootstrap-table.com/docs/api/table-options/#responsehandler
				*/ 
			   responseHandler:function(res){
				   console.log(res);
				   return {
					   total:res.obj.count,
					   rows:res.obj.data
				   }
			   },
			   /* 
					开启搜索 默认值为false,默认添加参数名为search
					官网地址: https://bootstrap-table.com/docs/api/table-options/#search
				*/
			   search:true,
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```

## 第七章 定制化搜索条件
### 第1节 定制化搜索(标签)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>定制化搜索条件</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		
		<div class="container">
			<div class="col-lg-8 col-lg-offset-2">
				<!--
					定制化搜索条件
					官网地址: https://examples.bootstrap-table.com/#options/custom-toolbar.html
				 -->
				 <div id="toolbar">
					 <form class="form-inline">
						 <!-- 下拉框(是否有效) -->
						<div class="form-group">
							<select name="isDelete" class="form-control" style="width: 85px;">
							  <option value="">全部</option>
							  <option value="1">有效</option>
							  <option value="0">无效</option>
							</select>
						</div>	
						<!-- 角色名 -->
						<div class="form-group">
							<input type="text" class="form-control" name="roleName" placeholder="角色名称">
						</div>
						<!-- 开始时间和结束时间 -->
						<div class="form-group">
							<input class="form-control" name="startTime" type="datetime-local">
							<input class="form-control" name="endTime" type="datetime-local">
						</div>
						<button id="search" type="button" class="btn btn-success">
						<span class="glyphicon glyphicon-search"></span>
						查询
						</button>
					 </form>
				 </div>
				
				<table
				  id="table"
				  data-toggle="table"
				  data-url="http://localhost:8080/searchRolePageParam"
				  data-pagination="true"
				  data-side-pagination="server"
				  data-pagination-pre-text="上一页"
				  data-pagination-next-text="下一页"
				  data-query-params="queryParams"
				>
				  <thead>
				    <tr>
				      <th data-field="roleId">角色编号</th>
				      <th data-field="roleName">角色名称</th>
				      <th data-field="createTime">创建时间</th>
					  <th data-field="updateTime">更新时间</th>
					  <th data-field="isDelete">是否删除</th>
				    </tr>
				  </thead>
				</table>
			</div>
		</div>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			
			$(function(){
				$("#search").click(function(){
					// 刷新获取首页数据
					$("#table").bootstrapTable('refresh');
				})
			})
			
			/*
			  处理请求参数
			 */
			function queryParams(params){
				// 获取定制化所有的条件参数,设置到params中(后台需要支持这些参数)
				$("#toolbar input[name]").each(function(){
					params[$(this).attr('name')]=$(this).val();
				});
				var select=$("#toolbar select>option:selected");
				params.isDelete = select[0].value;
				
				return params;
			}
			
		</script>
	</body>
</html>
```

### 第2节 定制化搜索(JS)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>分页结果集处理</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
		<div class="container">
			<div class="col-lg-8 col-lg-offset-2">
				<div id="toolbar">
				 <form class="form-inline">
					 <!-- 下拉框(是否有效) -->
					<div class="form-group">
						<select name="isDelete" class="form-control" style="width: 85px;">
						  <option value="">全部</option>
						  <option value="1">有效</option>
						  <option value="0">无效</option>
						</select>
					</div>	
					<!-- 角色名 -->
					<div class="form-group">
						<input type="text" class="form-control" name="roleName" placeholder="角色名称">
					</div>
					<!-- 开始时间和结束时间 -->
					<div class="form-group">
						<input class="form-control" name="startTime" type="datetime-local">
						<input class="form-control" name="endTime" type="datetime-local">
					</div>
					<button id="search" type="button" class="btn btn-success">
						<span class="glyphicon glyphicon-search"></span>
						查询
					</button>
				 </form>
				</div>
				<!-- 表格生成位置 -->
				<table id="table"></table>
			</div>
		</div>
		
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            //1. 获取table的JQuery对象
            var $table=$("#table");
			//获取搜索按钮对象绑定时间
			$("#search").click(function(){
				$table.bootstrapTable("refresh");
			});
            //2. 调用bootstrapTable函数
            $table.bootstrapTable({
                url:"http://localhost:8080/searchRolePageParam",
				/* 开启分页 */
				pagination:true,
				/* 从服务端获取数据 */
				sidePagination:"server",
				/* 上一页设置 */
				paginationPreText:"上一页",
				/* 下一页设置 */
				paginationNextText:"下一页",
				/* 
					参数设置 
				*/
				queryParams:function(params){
					// 获取定制化所有的条件参数,设置到params中(后台需要支持这些参数)
					$("#toolbar input[name]").each(function(){
						params[$(this).attr('name')]=$(this).val();
					});
					var select=$("#toolbar select>option:selected");
					params.isDelete = select[0].value;
					
					return params;
				},
                // 自定义表的列
                columns:[
                    {
                        // 自定义列名称
                        title:'角色ID',
                        // 后台JSON数据中对应的属性名
                        field:'roleId'
                    },
                    {
                        title:'角色名称',
                        field:'roleName'
                    },
                    {
                        title:'是否删除',
                        field:'isDelete'
                    }
                ]
            });
        </script>
    </body>
</html>
```
## 第八章 批量删除

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>批量删除</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<!-- 添加批量删除按钮 -->
		<div class="toolbar">
			<button id="batdel" class="btn btn-danger">批量删除</button>
		</div>
		<table
		  id="table"
		  data-toggle="table"
		  data-url="http://localhost:8080/searchRolePage"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-query-params="queryParams"
		  data-response-handler="responseHandler"
		>
		  <thead>
		    <tr>
			  <!-- 
				开启批量选择框
				官网地址: https://bootstrap-table.com/docs/api/column-options/#checkbox
			  -->
			  <th data-checkbox="true"></th>
		      <th data-field="roleId">角色编号</th>
		      <th data-field="roleName">角色名称</th>
			  <th data-field="createTime">创建时间</th>
			  <th data-field="updateTime">更新时间</th>
		      <th data-field="isDelete">是否删除</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			
			/*
				获取表和批量删除按钮的JQuery对象
			*/ 
		   var $table = $('#table');
		   var $batdel = $('#batdel');
		   //给批量按钮绑定事件
		   $batdel.click(function(){
			   /* 
					获取选中的数据
					官网地址: https://bootstrap-table.com/docs/api/methods/#getselections
				*/
			   var roles = $table.bootstrapTable("getSelections");
			   //console.log(roles);
			   //处理数据,只选择ID即可,将其设置到数组中
			   var roleIds=[];
			   for(var i=0;i<roles.length;i++){
				   roleIds.push(roles[i].roleId);
			   }
			   /**
				* AJAX调用后端
				* data数据要设置成json
				* 后端函数入参需要设置 (@RequestBody List<Integer> roleIds)
				*/
			   $.ajax({
				   url:'http://localhost:8080/batchDeleteRole',
				   method:'POST',
				   traditional:true,
				   contentType:'application/json',
				   data:JSON.stringify(roleIds),
				   success(d) {
				   	console.log(d);
					//刷新表格
					$table.bootstrapTable("refresh");
				   }
			   });
		   });
			
			/* 
				处理请求参数
			 */
			function queryParams(params){
				//设置isDelete请求参数
				params.isDelete=1;
				return params; //返回值将新的参数返回给bootstrap-table
			}
			/*
				处理结果集
			*/ 
		   function responseHandler(res){
			   /*
				  重新定义结果集,将不符合bootstrap-table的结果集修改成满足bootstrap-table的数据结构
			   */
			  var d={};
			  d.total=res.obj.count;
			  d.rows=res.obj.data;
			  return d;
		   }
		</script>
	</body>
</html>
```

## 第九章 列的定制化
### 第1节 返回值数据格式
```json
/* 返回值中有多层嵌套对象,bootstrap-table应该怎么处理 */ 
{
  "code": 0,
  "msg": "OK",
  "obj": {
    "pageNo": 1,
    "pageSize": 5,
    "hasPre": false,
    "hasNext": true,
    "count": 50,
    "pageCount": 10,
    "data": [
      {
        "productId": 3,
        "productName": "产品1",
        "categoryId": 2,
        "category": {
          "categoryId": 2,
          "categoryName": "分类1",
          "createTime": "2022-09-10 03:52:06",
          "updateTime": "2022-09-10 03:52:05",
          "isDelete": 1
        },
        "productSize": "300*101 mm",
        "productPrice": 101.01,
        "createTime": "2022-09-10 04:18:08",
        "updateTime": "2022-09-10 04:18:07",
        "isDelete": 1
      },
      {
        "productId": 5,
        "productName": "产品3",
        "categoryId": 2,
        "category": {
          "categoryId": 2,
          "categoryName": "分类1",
          "createTime": "2022-09-10 03:52:06",
          "updateTime": "2022-09-10 03:52:05",
          "isDelete": 1
        },
        "productSize": "300*103 mm",
        "productPrice": 103.01,
        "createTime": "2022-09-10 04:18:08",
        "updateTime": "2022-09-10 04:18:07",
        "isDelete": 1
      }
    ]
  }
}
```
### 第2节 列的数据定制化(标签)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>列的数据定制化</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<table
		  data-toggle="table"
		  data-url="http://localhost:8080/getProductList"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-response-handler="responseHandler"
		>
		  <thead>
		    <tr>
		      <th data-field="productId">产品编号</th>
		      <th data-field="productName">产品名称</th>
			  <!-- 
				此列的数据的json格式见第九章#第1节$1.1返回值数据格式
				此列展示分类名称,但是数据是一个嵌套数据,不能直接展示,需要对数据进行重新格式化(对象.属性方式获取嵌套数据)
				如果想对当前数据进行更复杂的定制化可以参考如下官网进行格式化
				官网地址: https://bootstrap-table.com/docs/api/column-options/#formatter
			   -->
			  <th data-field="category.categoryName" data-formatter="categoryFsormatter">产品分类</th>
			  <th data-field="productSize">产品尺寸</th>
			  <th data-field="productPrice">产品价格</th>
			  <th data-field="createTime">创建时间</th>
			  <th data-field="updateTime">更新时间</th>
		      <th data-field="isDelete">是否删除</th>
			  <th data-formatter="operateHandler">操作</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script>
			/*
				对当前表格单元格进行格式化
				value: 当前列的值
				row  : 当前行的值
				index: 当前行索引
				field: 当前行所处的列的列名
				返回值是当前行所处的列的内容
				还可以定制更新、删除按钮等操作
			*/ 
		   function categoryFsormatter(value,row,index,field){
			   // 在这里可以对当前列做一些样式更复杂的定制化
			   var s="<span><em style='color:green;'>"+value+"</em></span>"; 
			   return s;
		   }
		   /*
		   		定制化操作列
		   */ 
		   function operateHandler(value,row,index,field){
			   var s='<div style=\"width:100%;display:flex;justify-content:space-around;\"><button type="button" class="btn btn-danger btn-sm">删除</button>';
			   s+='<button type="button" class="btn btn-warning btn-sm">更新</button></div>';
			   return s;
		   }
			/*
				处理结果集
			*/ 
		   function responseHandler(res){
			  var d={};
			  d.total=res.obj.count;
			  d.rows=res.obj.data;
			  return d;
		   }
		</script>
	</body>
</html>
```
### 第3节 列的数据定制化(JS)
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>列的数据定制化</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            var $table=$("#table");
            $table.bootstrapTable({
                url:"http://localhost:8080/getProductList",
				pagination:true,
				sidePagination:"server",
				paginationPreText:"上一页",
				paginationNextText:"下一页",
			   responseHandler:function(res){
				   return {
					   total:res.obj.count,
					   rows:res.obj.data
				   }
			   },
                columns:[
                    {
                        title:'产品编号',
                        field:'productId'
                    },
                    {
                        title:'产品名称',
                        field:'productName'
                    },
					/*
						定制化嵌套数据列
						官网地址: https://bootstrap-table.com/docs/api/column-options/#formatter
					*/ 
					{
                        title:'产品分类',
                        field:'category.categoryName',
						formatter:function(value,row,index,field){
							var s="<strong style='color:red;'>"+value+"</strong>";
							return s;
						}
                    },
					{
                        title:'产品尺寸',
                        field:'productSize'
                    },
					{
                        title:'产品价格',
                        field:'productPrice'
                    },
					{
                        title:'创建时间',
                        field:'createTime'
                    },
					{
						title:'更新时间',
						field:'updateTime'
					},
                    {
                        title:'是否删除',
                        field:'isDelete'
                    },
					/*
						定制化操作列
						官网地址: https://bootstrap-table.com/docs/api/column-options/#formatter
					*/ 
					{
					    title:'操作',
						formatter:function(value,row,index,field){
							var s='<div style=\"width:100%;display:flex;justify-content:space-around;\"><button type="button" class="btn btn-danger btn-sm">删除</button>';
							s+='<button type="button" class="btn btn-warning btn-sm">更新</button></div>';
							return s;
						}
					}
                ]
            });
        </script>
    </body>
</html>
```
## 第十章 事件绑定
### 第1节 事件绑定(标签)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>列事件绑定</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
	</head>
	<body>
		<table
		  id="table"
		  data-toggle="table"
		  data-url="http://localhost:8080/getProductList"
		  data-pagination="true"
		  data-side-pagination="server"
		  data-pagination-pre-text="上一页"
		  data-pagination-next-text="下一页"
		  data-response-handler="responseHandler"
		>
		  <thead>
		    <tr>
		      <th data-field="productId">产品编号</th>
		      <th data-field="productName">产品名称</th>
			  <th data-field="category.categoryName" data-formatter="categoryFormatter">产品分类</th>
			  <th data-field="productSize">产品尺寸</th>
			  <th data-field="productPrice">产品价格</th>
			  <th data-field="createTime">创建时间</th>
			  <th data-field="updateTime">更新时间</th>
		      <th data-field="isDelete">是否删除</th>
			  <!-- 
				绑定事件
				官网地址: https://examples.bootstrap-table.com/#column-options/events.html
			  -->
			  <th data-formatter="operateHandler" data-events="operateEvents">操作</th>
		    </tr>
		  </thead>
		</table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<script> 
		   /*
				事件处理
				需要在定制化按钮样式的时候在其要绑定时间的html标签上添加类或者id选择器
				例如: 在operateHandler函数中生成按钮,这里添加了类选择器(分别的两个button中添加了del和edit)
				官网案例源码地址: https://examples.bootstrap-table.com/#column-options/events.html
		   */ 
		   window.operateEvents={
				/*
					定义删除事件
					'click .del'： 第一个参数事件名称 第二个参数 类选择器
				*/ 
				'click .del':function(e, value, row, index){
					console.log("row:",row);
					//AJAX调用服务器端删除接口,删除数据
					$.getJSON("http://localhost:8080/deleteProduct",{productId:row.productId},function(d){
						//删除当前行表格DOM或者刷新当前表格
						// $("#table").bootstrapTable('remove', {
						// 	field: 'productId',
						// 	values: [row.productId]
						// })
						$("#table").bootstrapTable("refresh");//刷新表格数据
					});
					
				},
				/*
					定义更新事件
				*/
				'click .edit':function(e, value, row, index){
					console.log("更新");
				}
		   }
		   function categoryFormatter(value,row,index,field){
			   var s="<span><em style='color:green;'>"+value+"</em></span>"; 
			   return s;
		   }
		   function operateHandler(value,row,index,field){
			   var s='<div style=\"width:100%;display:flex;justify-content:space-around;\"><button type="button" class="btn btn-danger btn-sm del">删除</button>';
			   s+='<button type="button" class="btn btn-warning btn-sm edit">更新</button></div>';
			   return s;
		   }
		   function responseHandler(res){
			  var d={};
			  d.total=res.obj.count;
			  d.rows=res.obj.data;
			  return d;
		   }
		</script>
	</body>
</html>
```
### 第2节 事件绑定(JS)
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>列事件绑定</title>
        <link rel="stylesheet" href="dist/css/bootstrap.css">
        <link rel="stylesheet" href="bt/bootstrap-table.min.css">
    </head>
    <body>
        <!-- 表格生成位置 -->
        <table id="table"></table>
        <script src="dist/js/jquery-3.5.1.min.js"></script>
        <script src="dist/js/bootstrap.js"></script>
        <script src="bt/bootstrap-table.js"></script>
        <script src="dist/js/bootstrap-table-zh-CN.js"></script>
        <script>
            var $table=$("#table");
            $table.bootstrapTable({
                url:"http://localhost:8080/getProductList",
				pagination:true,
				sidePagination:"server",
				paginationPreText:"上一页",
				paginationNextText:"下一页",
			    responseHandler:function(res){
				   return {
					   total:res.obj.count,
					   rows:res.obj.data
				   }
			   },
                columns:[
                    {
                        title:'产品编号',
                        field:'productId'
                    },
                    {
                        title:'产品名称',
                        field:'productName'
                    },
					{
                        title:'产品分类',
                        field:'category.categoryName',
						formatter:function(value,row,index,field){
							var s="<strong style='color:red;'>"+value+"</strong>";
							return s;
						}
                    },
					{
                        title:'产品尺寸',
                        field:'productSize'
                    },
					{
                        title:'产品价格',
                        field:'productPrice'
                    },
					{
                        title:'创建时间',
                        field:'createTime'
                    },
					{
						title:'更新时间',
						field:'updateTime'
					},
                    {
                        title:'是否删除',
                        field:'isDelete'
                    },
					{
					    title:'操作',
						formatter:function(value,row,index,field){
							var s='<div style=\"width:100%;display:flex;justify-content:space-around;\"><button type="button" class="btn btn-danger btn-sm del">删除</button>';
							s+='<button type="button" class="btn btn-warning btn-sm edit">更新</button></div>';
							return s;
						},
						/*
							事件绑定
							官网地址: https://bootstrap-table.com/docs/api/column-options/#events
						*/ 
						events:{
							'click .del':function(e, value, row, index){
								console.log("删除->",row);
							},
							'click .edit':function(e, value, row, index){
								console.log("编辑->",row);
							}
						}
					}
                ]
            });
        </script>
    </body>
</html>
```
## 第十一章 数据结构化(TreeGrid插件)
```html
bootstrap-table还添加了表格数据结构化的支持TreeGrid,但是需要额外添加 jquery-treegrid 库
官网地址: https://bootstrap-table.com/docs/extensions/treegrid/
```
### 第1节 数据结构
```json
[
  {
    "menuId": 1,
    "menuName": "顶级菜单标题1",
    "menuUrl": null,
    "level": 1,
    "parentId": 0,
    "createTime": "2022-09-17 14:55:38",
    "updateTime": "2022-09-17 14:55:38",
    "isDelete": 1
  },
  {
    "menuId": 2,
    "menuName": "顶级菜单标题2",
    "menuUrl": null,
    "level": 1,
    "parentId": 0,
    "createTime": "2022-09-17 14:55:38",
    "updateTime": "2022-09-17 14:55:38",
    "isDelete": 1
  },
  {
    "menuId": 3,
    "menuName": "顶级菜单标题3",
    "menuUrl": null,
    "level": 1,
    "parentId": 0,
    "createTime": "2022-09-17 14:55:38",
    "updateTime": "2022-09-17 14:55:38",
    "isDelete": 1
  },
  {
    "menuId": 4,
    "menuName": "顶级菜单标题4",
    "menuUrl": null,
    "level": 1,
    "parentId": 0,
    "createTime": "2022-09-17 14:55:38",
    "updateTime": "2022-09-17 14:55:38",
    "isDelete": 1
  },
  {
    "menuId": 5,
    "menuName": "顶级菜单标题5",
    "menuUrl": null,
    "level": 1,
    "parentId": 0,
    "createTime": "2022-09-17 14:55:38",
    "updateTime": "2022-09-17 14:55:38",
    "isDelete": 1
  },
  {
    "menuId": 6,
    "menuName": "子集菜单标题1",
    "menuUrl": null,
    "level": 2,
    "parentId": 1,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 7,
    "menuName": "子集菜单标题2",
    "menuUrl": null,
    "level": 2,
    "parentId": 1,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 8,
    "menuName": "子集菜单标题1",
    "menuUrl": null,
    "level": 2,
    "parentId": 2,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 9,
    "menuName": "子集菜单标题2",
    "menuUrl": null,
    "level": 2,
    "parentId": 2,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 10,
    "menuName": "子集菜单标题1",
    "menuUrl": null,
    "level": 2,
    "parentId": 3,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 11,
    "menuName": "子集菜单标题2",
    "menuUrl": null,
    "level": 2,
    "parentId": 3,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 12,
    "menuName": "子集菜单标题1",
    "menuUrl": null,
    "level": 2,
    "parentId": 4,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 13,
    "menuName": "子集菜单标题2",
    "menuUrl": null,
    "level": 2,
    "parentId": 4,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 14,
    "menuName": "子集菜单标题1",
    "menuUrl": null,
    "level": 2,
    "parentId": 5,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  },
  {
    "menuId": 15,
    "menuName": "子集菜单标题2",
    "menuUrl": null,
    "level": 2,
    "parentId": 5,
    "createTime": "2022-09-17 14:56:49",
    "updateTime": "2022-09-17 14:56:49",
    "isDelete": 1
  }
]
```
### 第2节 实现方式
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>数据结构化</title>
		<link rel="stylesheet" href="dist/css/bootstrap.css">
		<link rel="stylesheet" href="bt/bootstrap-table.min.css">
		<!-- 引入jquery.treegrid的css样式 -->
		<link href="https://cdn.jsdelivr.net/npm/jquery-treegrid@0.3.0/css/jquery.treegrid.css" rel="stylesheet">
	</head>
	<body>
		<!-- 
			结构化表格生成位置
			官网地址: https://bootstrap-table.com/docs/extensions/treegrid/
		-->
		<table id="table"></table>
		<script src="dist/js/jquery-3.5.1.min.js"></script>
		<script src="dist/js/bootstrap.js"></script>
		<script src="bt/bootstrap-table.js"></script>
		<script src="dist/js/bootstrap-table-zh-CN.js"></script>
		<!-- 引入jquery.treegrid的JS库 -->
		<script src="https://cdn.jsdelivr.net/npm/jquery-treegrid@0.3.0/js/jquery.treegrid.min.js"></script>
		<!-- 引入bootstrap-table-treegrid的JS库 -->
		<script src="https://unpkg.com/bootstrap-table@1.21.0/dist/extensions/treegrid/bootstrap-table-treegrid.min.js"></script>
		
		<script>
			var $table = $('#table');
			$(function() {
			    $table.bootstrapTable({
			      url: 'http://127.0.0.1:8080/getMenuList',
				  /* 主键ID,和父ID产生关联的属性 */
			      idField: 'menuId',
				  /*显示列*/
			      showColumns: true,
			      columns: [
			        {
			          checkbox: true
			        },
			        {
			          field: 'menuName',
			          title: '名称'
			        },
			        {
			          field: 'isDelete',
			          title: '状态',
					  formatter:function(value, row, index){
						 if (value === 1) {
						   return '<span class="label label-success">正常</span>'
						 }
						 return '<span class="label label-default">锁定</span>' 
					  }
			        }
			      ],
				  /* 列名称 */ 
			      treeShowField: 'menuName',
				  /* 父ID,与idField属性产生关联 */
			      parentIdField: 'pId',
			      onPostBody: function() {
			        $table.treegrid({
					  //在哪一列展示折叠图标,序号从0开始	
			          treeColumn: 1,
			          onChange: function() {
						//展示  
			            $table.bootstrapTable('resetView')
			          }
			        })
			      }
			    })
			  })
		</script>
	</body>
</html>
```
## 第十二章 其它配置
```html
1. 默认数据属性名rows修改 https://bootstrap-table.com/docs/api/table-options/#datafield
2. 默认数据库总行数total修改 https://bootstrap-table.com/docs/api/table-options/#totalfield
```