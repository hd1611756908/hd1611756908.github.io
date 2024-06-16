---
title: vue-admin-template快速入门
categories: vue
tags: ["html","css","JavaScript","vue","element","vue-element-admin","vue-admin-template"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://hd1611756908.github.io/
B 站: https://space.bilibili.com/514155929/
```

## 第一章 模板介绍

### 第1节 vue-element-admin概述

```
官网地址: https://panjiachen.github.io/vue-element-admin-site/zh/

是一个前端模版,可以帮助前端开发者快速构建管理系统的前端页面

模板中提供了 【登录 / 注销】【权限验证】【多环境发布】【全局功能】【编辑器/Excel】... 等常见功能

样式如下: ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
```



<!-- more -->



![vue](https://note.youdao.com/yws/api/personal/file/WEBd40f111969498907c275bd47dee662ec?method=download&shareKey=7d61f238d36c3e69aeb03a58cb0b48ae)

### 第2节 vue-admin-template概述

```xml
vue-admin-template是vue-element-admin的简洁版
在实际开发中,vue-element-admin里的很多功能都不是我们想要的
所以作者基于vue-element-admin进行了删减,vue-admin-template版本适合开发
如果想使用vue-element-admin里面的某些功能,可以去复制和粘贴.
```

### 第3节 源码地址

```
vue-element-admin地址:
	GITEE: https://gitee.com/panjiachen/vue-element-admin.git
	GITHUB: https://github.com/PanJiaChen/vue-element-admin.git
vue-admin-template地址:
	GITEE: https://gitee.com/panjiachen/vue-admin-template.git
	GITHUB: https://github.com/PanJiaChen/vue-admin-template.git
```

## 第二章 下载和运行

### 第1节 系统环境

```
我个人本地环境

操作系统: Windows 11 教育版
node:    v20.13.0   官网要求版本{ node: '8 || 10 || 12 || 14 || 16 || 17' } 但是v20.13.0也可以
npm:     10.5.2 
vue-cli: @vue/cli 5.0.8

npm包镜像站地址: https://npmmirror.com/

因为国外镜像源地址下载容易失败,可以切换成国内的镜像源

1. 查看镜像源命令:  npm config list
2. 设置镜像源命令:  npm config set registry https://registry.npmmirror.com

注意: 不允许使用cnpm进行依赖下载
```

### 第2节 依赖下载和运行

```
依赖下载和:
	第一步: 打开命令行工具
	第二步: 切换到项目的根目录
	第三步: 在命令行输入 npm install 命令
	第四步: 等待依赖安装完成
	第五步: 在命令行输入  npm run dev 启动运行
```

## 第三章 目录结构介绍

```
略 请参考 vue-cli脚手架的官网: https://cli.vuejs.org/zh/
```

## 第四章 左侧菜单名称以及图标修改

### 第1节 菜单名称修改

```
左侧菜单名称是在路由配置中生成的  文件位置: vue-admin-template\src\router\index.js

修改title属性的值就是对应着左侧菜单的名称显示
```

<img src="https://note.youdao.com/yws/api/personal/file/WEB962f59d88c36b6cb76394b70545dca67?method=download&shareKey=604d1722e6f45626d4a0c7052960f1d1" alt="vue-admin-02" />

### 第2节 菜单图标修改

```
左侧菜单中的图标是在路由配置中生成的  文件位置: vue-admin-template\src\router\index.js

修改icon属性的值就可以修改图标了
icon的值可以使用element-ui官网提供的图标显示,如果想定制化图标,可以参考官网: https://panjiachen.github.io/vue-element-admin-site/
```

<img src="https://note.youdao.com/yws/api/personal/file/WEBf8ff376348617ea8581d8eeda0e27a60?method=download&shareKey=9dacfa40d03d5f2a5b7b920ebb449b0f" alt="vue-admin-03" />

## 第五章 面包屑导航名称不一致修改

<img src="https://note.youdao.com/yws/api/personal/file/WEBe98538afb8c80609b3d5cfd32acafa18?method=download&shareKey=94619cd32d4775d24fb75811d88568cc" alt="vue-admin-04" />



```xml
修改菜单之后,面包屑导航菜单和首页名称不一致

修改文件位置: vue-admin-template\src\components\Breadcrumb\index.vue

修改如下: 👇👇👇👇👇
```

<img src="https://note.youdao.com/yws/api/personal/file/WEB74e3dddff3dec344e5e739cf566c10cf?method=download&shareKey=1c24776b5f88b62a913e70d492b69a52" alt="vue-admin-05" style="zoom:100%;" />

## 第六章 浏览器选项卡标题名称修改

![vue](https://note.youdao.com/yws/api/personal/file/WEBe986b0f5befbb3a2ae4802db83964e9e?method=download&shareKey=965e0019a774451ec1cb8c3d032960d0)

```
浏览器选项卡标题修改文件位置: vue-admin-template\src\settings.js
```

![vue](https://note.youdao.com/yws/api/personal/file/WEB5ee0c76c04b0c7e42559d025472e4985?method=download&shareKey=4bbd2df365848712edfdda7c43be0381)

## 第七章 标签导航实现

![vue](https://note.youdao.com/yws/api/personal/file/WEBb3db6adcab7bace31e1c1d4288943e71?method=download&shareKey=a85d41db232ccbab97511ccdf4f645d5)

---

![vue](https://note.youdao.com/yws/api/personal/file/WEB2ad5f7e8df21d9de6b0c0e00b9bf8b61?method=download&shareKey=5560c0ef3544410569a34c1e33966720)

```
vue-admin-template默认情况下是没有标签导航的,如果想实现标签导航,需要去vue-element-admin项目中复制

步骤如下:

第一步: 去vue-element-admin项目中复制标签导航的功能组件
	文件位置: vue-element-admin\src\layout\components\TagsView 
	将TagsView文件夹整个复制到vue-admin-template对应的目录中

第二步: 复制配置文件
	文件位置:
		vue-element-admin\src\store\modules\tagsView.js
		vue-element-admin\src\store\modules\permission.js
	复制到vue-admin-template对应的目录下

第三步: 修改配置文件,详细修改如下
```

* 第一个文件 vue-element-admin\src\layout\index.vue

<img src="https://note.youdao.com/yws/api/personal/file/WEB2bcbcb16ad68501a928cf847152d8288?method=download&shareKey=d35ab066bdf54f4d99d5c6918e91a30b" alt="vue" style="zoom:80%;" />

* 第二个文件 vue-admin-template\src\layout\components\index.js

<img src="https://note.youdao.com/yws/api/personal/file/WEBf0c4d438dedb62e0faaf472f3d66fc8c?method=download&shareKey=68ee161ce86652471902dc95533bd546" style="zoom:80%;" />

* 第三个文件 vue-admin-template\src\store\index.js

<img src="https://note.youdao.com/yws/api/personal/file/WEBc656dfc0b1bc9e77a5e43e8354857c34?method=download&shareKey=033f68d21834c7becdb6d077e9e536ec" alt="vue" style="zoom:100%;" />

* 第四个文件 vue-admin-template\src\store\getters.js

```
这个文件内容可以去vue-element-admin中去复制
```

![vue](https://note.youdao.com/yws/api/personal/file/WEB29d834311db51cea37fff3245da1b57b?method=download&shareKey=25ccc629ad1673ebad5f76982552445f)

* 第五个文件  vue-admin-template\src\layout/components/AppMain.vue

```
这个文件的内容可以去vue-element-admin中去复制
```

![vue](https://note.youdao.com/yws/api/personal/file/WEBb3e5a2b8f017222d155f1f0b3dcf1735?method=download&shareKey=2bf7c6b5cdbeec73d4af361425ac6dec)

* 第六个文件(钉) vue-admin-template\src\router\index.js

```
这个一定要配置,否则当标签实现的时候，当用户把标签全部删除没了之后会报404,所以必须得留下一个标签,所以需要钉一下

这个需要看你需要将哪个标签固定下来,关闭不掉即可.配置方式见下图
```

![vue](https://note.youdao.com/yws/api/personal/file/WEBa7ca763e0c87ebbba76a39f2cc2fc1d3?method=download&shareKey=3dd1741793fcbe71a6fff00d42e0102f)

```
设置affix: true 参数，首页就会被固定住,此标签不会被关闭掉. 见下图
```

![vue](https://note.youdao.com/yws/api/personal/file/WEBe5e415d4c88cbbba965c27d05d4b7f13?method=download&shareKey=7494b431a3f7ab7b333fb875f624d9a6)

## 第八章 标签导航经控制台警告处理

![vue](pic\vue-admin-17.png)

```
控制台出现警告的原因是因为在 vue-admin-template\src\store\modules\permission.js 文件中没有找到asyncRoutes变量

这个变量需要在vue-admin-template\src\router\index.js文件中声明,所在此文件中暂时先声明一个asyncRoutes变量,是一个数组

export const asyncRoutes = [] 可以解决,后面在说这个变量的意义.
```

