---
title: nginx笔记
categories: nginx
tags: ["nginx","反向代理","正向代理","web服务器"]
---

```
Author: 夜泊
Email : hd1611756908@163.com
QQ    : 1611756908
web   : https://ukoko.gitee.io
sign  : 每走一步,都是进步
```


### 一、Nginx简介

> Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的。其特点是占有内存少，并发能力强。中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等

* 官网地址

> http://nginx.org/

* 版本介绍

> Mainline version : 主线版本
>
> Stable version   : 稳定版本
>
> Legacy versions  : 历史版本

---

<!--more-->

### 二、Nginx安装

* nginx windows目录简介

<center>

<img src="https://note.youdao.com/yws/api/personal/file/WEB9dc661b15ac46905518f79034d29483e?method=download&shareKey=a8c242e0ec3a5d9ccf00abf30e1f5046" />

</center>

* windows安装

* * 安装步骤

  > 1、解压下载的nginx压缩包(所有的配置默认)
  >
  > 2、启动命令行切换到当前nginx解压目录(nginx.exe目录下)
  >
  > 3、使用命令行启动  start nginx.exe
  >
  > 4、有一个一闪而过的效果
  >
  > 
  >
  > windows环境下nginx常见命令:
  >
  > 1、启动 start nginx / nginx.exe  
  >
  > 2、停止 nginx.exe -s stop    /  nginx.exe -s quit   (stop快速停止/quit有序停止)
  >
  > 3、重载 nginx.exe -s reload  （主要用于配置修改后的重新载入）
  >
  > 4、查看nginx版本 nginx -v

  * 注意事项

  > 不要直接双击nginx.exe，这样会导致修改配置后重启、停止nginx无效，需要手动关闭任务管理器内的所有nginx进程

  * 成功展示

 <center>
 
 <img src="https://note.youdao.com/yws/api/personal/file/B5F24D3E19C04C6C99148F3D49FA5F6B?method=download&shareKey=6599957b13da3e33269a7b859280bec7"/>

 </center>

* Linux安装

> 1、下载
> 	wget  https://nginx.org/download/nginx-1.11.6.tar.gz
>
> 2、相关依赖库安装(Linux下nginx安装必备的安装库)
>
> - 2.1 gcc环境                 yum install gcc-c++
> - 2.2 PCRE依赖库              yum install -y pcre pcre-devel
> - 2.3 zlib 依赖库             yum install -y zlib zlib-devel
> - 2.4 OpenSSL安全套接字层密码库 yum install -y openssl openssl-devel
>
> 3、解压nginx压缩包   
>
> ​	tar -zxvf nginx-1.11.6.tar.gz
>
> 4、进入nginx解压目录执行 
> 	./configure
>
> 5、安装
> make install
>
> 6、查找安装路径
> whereis nginx
> 这个命令可以在Linux环境下查找到当前nginx安装到了哪里.
>
> 7、启动/停止nginx
>
> - nginx的命令默认在不指定安装路径的情况下会安装到 /usr/local/nginx/sbin
>
> - Linux下nginx常用命令
>
>    ./nginx 开启
>    ./nginx -s stop 停止
>    ./nginx -s quit
>    ./nginx -s reload
>
> 8、配置文件所在目录
>
> ​	/usr/local/nginx/conf

* 在docker平台安装nginx

> 1、在docker搜索当前的nginx镜像  docker search nginx
>
> 2、拉取当前最新版本             docker pull nginx:latest
>
> 3、查看本地镜像                 docker images
>
> 4、运行nginx容器               docker run --name nginx-app -p 8081:80 -d nginx
> 	--name nginx-app : 给容器起名字
> 	-p 8081:80       : 端口映射,将本地 8081 端口映射到容器内部的 80 端口
> 	-d nginx         : 设置容器在在后台一直运行
>
> 5、访问测试,看是否安装成功

---

### 三、Nginx配置

#### 3.1 配置文件的构成

```xml
# 全局块
...              
# events块
events {         
   ...
}
# http块
http      
{
    # http全局块
    ...   
    # 虚拟主机server块
    server        
    { 
        # server全局块
        ...       
        # location块
        location [PATTERN]   
        {
            ...
        }
        location [PATTERN] 
        {
            ...
        }
    }
    server
    {
      ...
    }
    # http全局块
    ...     
}
```

#### 3.2 配置文件详解

```xml
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象①发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll(处理请求效率更高)|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。

    # 定义常量
    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页 

    #定义某个负载均衡服务器   
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
} 
```

① 惊群效应

> 惊群效应（thundering herd）是指多进程（多线程）在同时阻塞等待同一个事件的时候（休眠状态），如果等待的这个事件发生，那么他就会唤醒等待的所有进程（或者线程），但是最终却只能有一个进程（线程）获得这个时间的“控制权”，对该事件进行处理，而其他进程（线程）获取“控制权”失败，只能重新进入休眠状态，这种现象和性能浪费就叫做惊群效应

---

### 四、Nginx反向代理

#### 4.1 什么是反向代理

> 在说反向代理之前首先我们要先说一下什么是正向代理

* 正向代理概念

> 正向代理指的是，一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端

<center>

<img src="https://note.youdao.com/yws/api/personal/file/DE41E2E36B79423386D3A844608A36B5?method=download&shareKey=71871444dfe5c7bc9ab62358d2d5245b" />

</center>



* 反向代理概念

> 反向代理（Reverse Proxy）方式是指以代理服务器来接受Internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给Internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器

<center>

<img src="https://note.youdao.com/yws/api/personal/file/15CF5B742EA54457BC9680C71A91AE40?method=download&shareKey=732ca220deb994e6294ef5b7bdbdf8c6"/>

</center>



> 怎么区分正向代理和反向代理:
>
> 1. 首先代理是一种设计模式
> 2. 服务器采用这样的设计结构实现了这种设计模式
> 3. 正向代理，代理的是客户端,正向代理将代理服务器看成了一个客户端，让这个代理服务器做客户端的功能(为客户端收发请求)
> 4. 反向代理，代理的是服务器(就像是服务器一样,为服务器收发请求)
> 5. 正向代理是代理服务器和用户在一个LAN(Local Area Network)内
> 6. 反向代理是代理服务器和目标服务器在一个LAN内

#### 4.2 为什么要用反向代理

> 1、可以起到保护网站安全的作用，因为任何来自Internet的请求都必须先经过代理服务器
>
> 2、通过缓存静态资源，加速Web请求
>
> 3、实现负载均衡

#### 4.3 反向代理的实现

> 配置反向代理非常简单，只需要在location块中设置代理服务器地址即可

```
location /ceshi {
    # 代理服务器地址
    proxy_pass   http://localhost:8080/PictureSharing/index; # 网站的首页地址（也可以是某一个请求的地址）
}
```

---

###  五、Nginx负载均衡

* 负载均衡的基本配置

> 负载均衡实现:

```
#设置负载均衡upstream块
upstream mysvr1 {   
	server 127.0.0.1:8080;
	server 127.0.0.1:8081;
}


如果负载均衡不生效，切换浏览器,使用其它浏览器(谷歌浏览器负载均衡可能会失效)
```

> 设置路由规则

```
# 设置路由规则(当设置负载均衡时,不能在location后设置规则和拦截路径)
location / {
    proxy_pass  http://mysvr1;
}
```

> 负载均衡设置以上两个块即可,但是要注意upstream块在http块内部和server块同级.
> upstream : 设置upstream块
> mysvr1   ：给upstream起一个名字，在location块里面设置负载均衡服务器群组的upstream块的名字,然后重载nginx配置

* 负载均衡的策略

> 热备: 如果有2台服务器，当一台服务器发生事故时，才启用第二台服务器给提供服务。服务器处理请求的顺序：AAAAAA突然A挂啦，BBBBBBB

```xml
upstream mysvr1 {   
	server 127.0.0.1:8080;
	server 127.0.0.1:8081 backup;  #热备 
}
```

> 轮询 : nginx默认就是轮询其权重都默认为1，服务器处理请求的顺序：ABABABABAB

```
upstream mysvr1 {   
	server 127.0.0.1:8080;
	server 127.0.0.1:8081;
}
```

> 加权轮询: 根据配置的权重的大小而分发给不同服务器不同数量的请求。如果不设置，则默认为1。下面服务器的请求顺序为：ABBABBABBABBABB

```
upstream mysvr1 {   
	server 127.0.0.1:8080 weight=1;
	server 127.0.0.1:8081 weight=2;
}
```

> ip_hash: nginx会让相同的客户端ip请求相同的服务器

```
upstream mysvr1 {   
	server 127.0.0.1:8080;
	server 127.0.0.1:8081;
	ip_hash;
}
```

> 关于负载均衡的几个常见其他参数(了解)
>    - down         : 表示当前的server暂时不参与负载均衡
>    - max_fails    : 允许请求失败的次数，默认为1。当超过最大次数时，返回proxy_next_upstream 模块定义的错误
>    - fail_timeout : 在经历了max_fails次失败后，暂停服务的时间。max_fails可以和fail_timeout一起使用 

```xml
upstream mysvr { 
    server 127.0.0.1:8080 weight=2 max_fails=2 fail_timeout=2;
    server 127.0.0.1:8081 weight=1 max_fails=2 fail_timeout=1;    
}
```

---

### 六、Nginx静态资源访问

> 使用Nginx搭建图片服务器

#### 6.1 配置图片服务器的步骤

> 配置图片服务器只需要修改nginx.cnf配置文件中的location块即可

```
#图片服务器的地址设置
location /file/ {
	alias  D:/360Downloads/wpcache/srvsetwp/; # 指定根目录
	autoindex on;                             # 文件可以以列表的形式展示出来
}
```

> 访问:  http://localhost/file   就会展示出来图片列表

#### 6.2 root 和  **alias** 区别

> nginx指定文件路径有两种方式root和alias，这两者的用法区别在于对url的处理方法不同

* 方式一

```
location /file/ {
	alias  D:/360Downloads/wpcache/srvsetwp/; # 指定根目录
	autoindex on;                             # 文件可以以列表的形式展示出来
}
```

> 访问地址  http://127.0.0.1/file/ 时，ningx会自动去D:/360Downloads/wpcache/srvsetwp/目录找文件

* 方式二

```
location /file/ {
	root  D:/360Downloads/wpcache/srvsetwp/; # 指定根目录
	autoindex on;                             # 文件可以以列表的形式展示出来
}
```

> 访问地址  http://127.0.0.1/file/ 时，ningx会自动去D:/360Downloads/wpcache/srvsetwp/file 目录找文件

> 注意事项:
> 1. alias是一个目录别名的定义，root则是最上层目录的定义，file拼接到整个root指定的目录结尾
> 2. alias后面必须要用"/"结束，否则会找不到文件的,而root则可有可无

---

### 七、Nginx动静分离

####  7.1 动静分离介绍

> 为了提高网站的响应速度，减轻程序服务器（tomcat，jetty等）的负载，对于静态资源比如html、css、js、图片等文件，我们可以在反向代理服务器中进行缓存，这样浏览器在请求一个静态资源时，代理服务器就可以直接处理，而不用将请求转发给后端服务器

<center>

<img src="https://note.youdao.com/yws/api/personal/file/390C38FD102045438B398739A790587B?method=download&shareKey=c96fc3f161d972cb336ecc6cf6c5df62" />

</center>


#### 7.2 动静分离实现

* 将前端页面都放在nginx中并配置

> 实现方式:
>
> 1、配置静态资源文件  自定义一个index.html文件做测试

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>首页</title>
        <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
    </head>
    <body>
        <h1>Nginx前后端分离测试</h1>
        <button id="btn">向后台发送请求</button>
        <script>
            $("#btn").click(function(){
                //请求地址访问web服务器的地址
                $.getJSON("http://localhost:8080/getList", function(json){
                    console.log(json)
                });
            });
        </script>
    </body>
</html>
```

> 2、将index.html页面放在硬盘任意位置，比如我放在了 D:\app\hs_admin，配置nginx，将index.html 设置为nginx的首页

```xml
location /abc {
	alias D:/app/hs_admin/;
	index index.html;
}
```

> 3、启动nginx服务器,在浏览器中输入 http://localhost/abc/ 测试nginx服务器首页是否设置成功

* 使用SpringBoot构建一个web项目用于前后端分离项目测试

```java
//后台代码
@RestController
public class HelloController {
    @GetMapping(value = "/getList")
    public Object getList(){
        List<Map<String,String>> listMap = new ArrayList<>();
        Map<String,String> map1 = new HashMap<>();
        map1.put("userName","李雷");
        map1.put("age","18");
        Map<String,String> map2 = new HashMap<>();
        map2.put("userName","韩梅梅");
        map2.put("age","17");
        listMap.add(map1);
        listMap.add(map2);
        return listMap;
    }
}
```

* 测试

> 当项目设计成前后端分离之后,那么我们前后端的数据交互需采用JavaScript进行AJAX请求,我这里采用的是jQuery进行测试
>
> 在nginx服务器中的index.html页面请求后台的web服务器中的资源会出现跨域调用的问题(No 'Access-Control-Allow-Origin' header is present on the requested resource)

#### 7.3 跨域问题解决

* 什么是跨域

> 浏览器的同源策略会导致跨域，跨域是指从一个域名的网页去请求另一个域名的资源。比如从www.baidu.com 页面去请求 www.google.com 的资源。跨域的严格一点的定义是：只要 协议(http/https)，域名，端口号有任何一个的不同，就被当作是跨域，浏览器因为同源策略不允许AJAX从当前域请求到另一域获取数据

* 解决方式(Spring框架,专注后台解决方案)

> 只要在请求的Java类(控制器类)上加@CrossOrigin注解即可
>    - @CrossOrigin注解是Spring在4.2版本新增的一个注解
>    - @CrossOrigin注解可以修饰类也可以修饰方法
>    - @CrossOrigin默认是可以接受其他域的所有请求
>    - 还可以针对某一个域进行设置,此注解里面有一个属性
>       	 - origins : 可以设置指定的域名或方法  例如: origins = "http://localhost"