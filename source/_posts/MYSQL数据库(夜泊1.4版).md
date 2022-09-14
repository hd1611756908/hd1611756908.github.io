---
title: MySQL数据库
categories: MYSQL
tags: ["MySQL","SQL"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://ukoko.gitee.io/
B 站: https://space.bilibili.com/514155929/
```

## 第一章 数据库简介

### 第1节 数据库的概念

数据库（DataBase,DB）：是以一定方式储存在一起、能与多个用户共享、与应用程序彼此独立的数据集合，可视为电子化的文件柜——存储电子文件的处所，用户可以对文件中的数据进行新增、查询、更新、删除等操作。

---

<!--more-->

### 第2节 数据库管理系统概念

数据库管理系统（DataBase Management System，DBMS）：指一种操作和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制，以保证数据库的安全性和完整性。用户通过数据库管理系统访问数据库中的数据。

---

### 第3节 常见的数据库管理系统
* Oracle：Oracle数据库被认为是业界目前比较成功的关系型数据库管理系统。Oracle数据库可以运行在UNIX、Windows等主流操作系统平台，完全支持所有的工业标准，并获得最高级别的ISO标准安全性认证。
* MySQL：MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle旗下产品。MySQL是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件。
* DB2：DB2是IBM公司的产品，DB2数据库系统采用多进程多线索体系结构，其功能足以满足大中公司的需要，并可灵活地服务于中小型电子商务解决方案。
* Microsoft SQL Server：SQL Server 是Microsoft 公司推出的关系型数据库管理系统。具有使用方便可伸缩性好与相关软件集成程度高等优点。

---

## 第二章 数据库的安装

### 第1节 window下MYSQL数据库安装/卸载
安装:(略)

卸载:
```
1. 关闭服务
2. 通过360或者其他卸载软件卸载mysql服务器
3. 通过运行窗口使用 regedit.exe 命令进入注册表
4. 清理注册表
A.HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQL 目录
B.HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQL 目录
C.HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQL 目录
D.HKEY_LOCAL_MACHINE\SYSTEM\CurrentControl001\Services\MYSQL 目录
E.HKEY_LOCAL_MACHINE\SYSTEM\CurrentControl002\Services\MYSQL　目录
F.HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MYSQL　目录
5. 删除C:\Documents and Settings\All Users\Application Data\MySQL 目录 （隐藏的目录）
6. 删除安装路径文件，重新安装新版MYSQL 
7. 查询windows服务列表,找到服务名称,在cmd命令行使用sc delete 服务名 删除服务
注意: 如果上面的步骤在删除时找不到，那么略过。
```

---

## 第三章 SQL语句

### 第1节 SQL语句概述
SQL：Structure Query Language（结构化查询语言），SQL被美国国家标准局（ANSI）确定为关系型数据库语言的美国标准，后来被国际化标准组织（ISO）采纳为关系数据库语言的国际标准.
```
各数据库厂商都支持ISO的SQL标准 --> 普通话
各数据库厂商在标准的基础上做了自己的扩展 --> 方言

SQL 是一种标准化的语言，它允许你在数据库上执行操作.
  --> 如创建项目，查询内容，更新内容，并删除条目等操作.
```
注意事项:
```
1、SQL 语言大小写不敏感（一般情况关键字使用大写）。
2、SQL 可以写在一行或者多行
3、关键字不能被缩写也不能分行
4、各子句一般要分行写
5、使用缩进提高语句的可读性
```

---

### 第2节 SQL语句分类
* DDL（Data Definition[ˌdefɪˈnɪʃn] Language）：数据定义语言，用来定义数据库对象：库、表、列等。
* DML（Data Manipulation[məˌnɪpjʊˈleɪʃən] Language）：数据操作语言，用来定义数据库记录（数据）。
* DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别。
* DQL（Data Query Language）：数据查询语言，用来查询记录数据

---

### 第3节 DDL(数据定义语言[Data Definition Language])使用

#### 3.1 创建数据库
```
CREATE DATABASE [库名称] DEFAULT character set utf8;
```
#### 3.2 查看当前使用的数据库
```
SELECT database();
```
#### 3.3 切换数据库
```
use [库名称]
```
#### 3.4 删除数据库
```
DROP DATABASE [库名称]
```
#### 3.5 创建数据库表
```
CREATE TABLE [表名称](
    id BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT '主
键',
    email VARCHAR(255) NOT NULL UNIQUE COMMENT '邮箱，唯一',
    name VARCHAR(255) NOT NULL COMMENT '名称',
    create_time DATETIME NOT NULL COMMENT '创建时间',
    update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '设置当前时间，并且自动更更新时间'
) ENGINE=InnoDB AUTO_INCREMENT=1000 DEFAULT charset=utf8 COMMENT='表的描述';
```
常用的数据类型:
```
INT：整型
BIGINT: 长整形
DOUBLE：浮点型，例如DOUBLE(5,2)表示最多5位，其中必须有2位小数，即最大值为999.99；
CHAR：固定长度字符串类型； CHAR(10) 'aaa ' 占10位
VARCHAR：可变长度字符串类型； VARCHAR(10) 'aaa' 占3位
TEXT：字符串类型；
BLOB：字节类型；
DATE：日期类型，格式为：yyyy-MM-dd；
TIME：时间类型，格式为：hh:mm:ss
TIMESTAMP：时间戳类型 yyyy-MM-dd hh:mm:ss 会自动赋值
DATETIME:日期时间类型 yyyy-MM-dd hh:mm:ss
```
常用约束:
```
主键约束：PRIMARY KEY
唯一约束：UNIQUE [KEY]
非空约束：NOT NULL
默认约束：DEFAULT
外键约束：FOREIGN KEY
自动增长：AUTO_INCREMENT
```


#### 3.6 显示表结构
```
SHOW CREATE TABLE [表名称]\G
```
```
DESC [表名字]
```
#### 3.7 删除表
```
DROP TABLE [表名称]
```
#### 3.8 更新表的列的约束条件
```
ALTER TABLE [表名] modify [列名] [修改的条件]
```
#### 3.9 修改表的名称和约束条件
```
ALTER TABLE [表名] change [源列名] [新列名] [约束条件]
```
#### 3.10 添加表的列
```
ALTER TABLE [表名] add [列名] [约束条件]
```
#### 3.11 删除表的列
```
ALTER TABLE [表名] DROP [列名]
```
#### 3.12 创建表中列的索引
```
ALTER TABLE [表名] add index [自定义索引名称] (表中需要添加索引的列名)

eg: ALTER TABLE user ADD INDEX solr_key_ (name)
```
#### 3.13 删除索引
```
ALTER TABLE [表名] DROP index [自定义的索引名称]

eg: ALTER TABLE user DROP INDEX solr_key_;
```

#### 3.14 添加外键约束(多表之间的关系)
在实际开发中，项目不可能设计一张表,肯定会有多张表,多张表之间的数据是有关联关系的，怎么实现表与表之间的关联关系有哪些？表与表之间的关联关系怎么实现？
```
3.13.1 一对多关联关系
eg:
比如说一个用户下有多个订单  user表(1)-->order表(n)
比如一个部门里面有多名员工  department表(1)-->employee表(n)

一对多创建表的原则:
① 在多个一方创建字段
② 字段作为外键指向一的一方

考虑如果反着创建外键(在一的一方创建外键)会出现什么情况?

3.13.2 多对多关联关系
eg: 
比如一个商品属于多个订单,一个订单下可以有多个商品,商品和订单就是一个典型的多对多关系。

多对多创建表的原则：
① 需要创建第三张表
② 中间表中至少两个字段
③ 这两个字段分别作为外键指向各自一方的主键

3.13.3 一对一关联关系(实际应用不是很多)
eg: 
比如一个人对应一个身份证号

一对一创建表的原则(两种):
① 基于外键约束(将一对多的关联关系的外键去唯一约束unique)
② 基于主键约束(让一对一双方的主键建立关系)

```


一对多的具体实现(表已经存在/表不存在的创建方式):
* 表已经存在，给已经存在的表添加外键 举例-两张表: table1 :Persons
```
CREATE TABLE Persons(
    id_p bigint not null primary key AUTO_INCREMENT COMMENT '主键',
    last_name varchar(255) not null COMMENT '名称'
);
```
table2:Orders
```
CREATE TABLE Orders(
    id_o bigint not null primary key AUTO_INCREMENT COMMENT '主键',
    order_no bigint not null COMMENT '订单号',
    id_p bigint not null COMMENT '外键指向Persons表的主键id_p'
);
```
给Orders表id_p添加外键约束,关联Persons表的主键id_p
```
ALTER TABLE Orders ADD FOREIGN KEY (id_p) REFERENCES Persons(id_p);
```
* 当表不不存在时，在创建表的时候直接添加外键需求和上面的相同还是两张表，这次在创建表的时候开始就进行行外键关联，Orders表的外键id_p关联Persons表的主键id_p 

table1： Persons
```
CREATE TABLE Persons(
    id_p bigint not null primary key AUTO_INCREMENT COMMENT '主键',
    last_name varchar(255) not null COMMENT '名称'
);
```
table2:Orders
```
CREATE TABLE Orders(
    id_o bigint not null primary key AUTO_INCREMENT COMMENT '主键',
    order_no bigint not null COMMENT '订单号',
    id_p bigint not null COMMENT '外键指向Persons表的主键id_p',FOREIGN KEY (id_p) REFERENCES Persons(id_p)
);
```

多对多具体实现:
```
# 商品表
CREATE TABLE `products` (
    `pid` varchar(32) PRIMARY KEY,
    `name` VARCHAR(40),
    `price` double
);

#订单表
create table `orders`(
    `oid` varchar(32) PRIMARY KEY,
    `totalprice` double
);

# 中间表
create table orderitem(
    `oid` varchar(50),
    `pid` varchar(50)
);
# 设置中间表两个字段为联合主键
ALTER TABLE `orderitem` add PRIMARY KEY (oid,pid);

# 将订单表的主键和商品表的主键分别于中间表的oid和pid相关联
ALTER TABLE `orderitem` ADD FOREIGN KEY (oid) REFERENCES orders(oid);
ALTER TABLE `orderitem` ADD FOREIGN KEY (pid) REFERENCES products(pid);

```

一对一具体实现:
```
# 用户表
create table user(
    `uid` int not null primary key auto_increment,
    `uname` varchar(20) not null,
    `cid` int not null unique comment '外键约束,关联card表主键'
);

# 身份证表
create table card(
    `cid` int not null primary key auto_increment,
    `caddress` varchar(20) not null
);

# 基于外键关联
① 在哪一方设置外键都可以,比如我在user设置外键cid
② 添加外键和约束之后这是一个一对多关联
③ 给外键列设置唯一约束 unique
ALTER TABLE `user` ADD FOREIGN KEY (cid) REFERENCES card(cid);

# 基于主键关联
ALTER TABLE `user` ADD FOREIGN KEY (uid) REFERENCES card(cid);

注意:自增的主键放在被关联表的一方
```

#### 3.15 撤销外键约束
```
ALTER TABLE Orders DROP FOREIGN KEY Orders_ibfk_1
```
注意:Orders_ibfk_1 是我们添加外键的时候由于我们没有指定外键名称，数据库就会默认生成一个外键名称，可以通过SHOW CREATE TABLE [表名称]\G 命令查看生成的外键的名称，然后在使用上面的命令进行删除Orders表的外键操作。

---

### 第4节 DML(数据操纵语言[Data Manipulation Language])操作

#### 4.1 INSERT(插入)
```
INSERT INTO [表名] ([列名1,列名2,列名3,列名4,……])
VALUES([列值1,列值2,列值3,列值4,……]),([列值1,列值2,列值3,列值4,……]),……
```
```
eg: INSERT INTO user(user_name,password) VALUES("李雷","123456"),("韩梅梅","111111");
```
#### 4.2 UPDATE(更新)
```
UPDATE [表名称] SET [列名1]=[列值1],[列名2]=[列值2],
…… [限制条件] WHERE [列名]=[实参] AND [OR] [条件参数...]
```
```
eg：UPDATE user SET user_name="李雷1",password="222222" WHERE user_id=100;
```
#### 4.3 DELETE(删除)
```
DELETE FROM [表名称] WHERE [列名]=[实参] AND [OR] [条件参数...]
```
```
eg: DELETE FROM user WHERE user_id=101;
```

---

### 第5节 DCL(数据控制语言[Data Control Language])使用
* GRANT：授予访问权限（在第五章数据库管理详细介绍）
* REVOKE：撤销访问权限（在第五章数据库管理详细介绍）
* COMMIT：提交事务处理
```
->$ commit;
查询当前数据库事务是否是自动提交,如果是自动提交那么，不需要手动commit();

查询当前MYSQL数据库是否为自动提交:
当前会话: show variables like '%autocommit%';
全局: show global variables like 'autocommit';

设置当前MYSQL数据库的自动/非自动提交:
当前会话: set session autocommit=1/0;
全局: set global autocommit=1/0;

```
* ROLLBACK：事务处理回退
```
->$ rollback;
```

---

## 第四章 DQL(数据查询语言[Data Query Language])数据查询

```
测试数据导入: 使用navcat客户端进行测试数据导入

数据地址:百度云网盘,自行下载

链接: https://pan.baidu.com/s/1ubnhJX5caNTm2kSPkYnY1A
提取码: tnpj

如果链接失效 QQ 1611756908 联系我
```
### 第1节 基础语法介绍
```
SELECT [列名:a,b,c] FROM [表名:a,b,c] [限制条件……]

SELECT: 标识选择哪些列
FROM ： 标识从哪些个表中选择

```
### 第2节 其他常用语法介绍
```
1.算数运算符 + - * /
2.取别名:可以给列和表取别名
				--> 直接取别名在列名的后面  last_name ln
				--> 使用as取别名         last_name as ln
				--> 使用双引号""         last_name "ln"


3.过滤和排序
				--> 限制条件 多表联查的时候如果不加限制条件会出现笛卡尔积的问题
				--> 排序 order by :一般情况放在结果集最后进行排序 默认升序ASC 降序:DESC

4.运算符 >, <, <=, >=, <>(!=)

5.其他运算符 
				--> BETWEEN……AND…… 包含边界
				--> IN([参数])
				--> IS NULL
				--> LIKE : 模糊匹配 _:代表任意一个 %:任意一个或多个

6.逻辑运算符:
				--> AND
				--> OR
				--> NOT



7.多表查询
				--> 等值连接
				--> 非等值连接
				--> 多个连接条件用AND [OR] 操作符
				--> 连接分类
					--内连接
					--外连接
					--满连接



8.分组函数
				--> avg()：求平均值
				--> sum()：求和
				--> max()：求最大值
				--> min()：求最小值
				--> count()：求总数

9. 组函数 group by(分组函数)
				--> 所有包含于SELECT 列表中，而未包含于组函数中的列都必须包含于 GROUP BY 子句中
				注意: WHERE 子句后不能使用组函数,如果使用组函数使用HAVING子句


10.子查询
				--> 子查询要放在括号()中
				--> 子查询要放在查询条件的右侧
				--> 查询结果集要一对一，多对多
```
### 第3节 查询语法介绍

#### 3.1 查询全部列
```
SELECT * FROM departments;

匹配当前表中所有的列(通配符，通配全部)
```
#### 3.2 查询选择特定的列
```
SELECT department_id, location_id FROM departments;

通过具体的列名查询
```
#### 3.3 算数运算符
```
算数运算符  + - * /  优先级先算乘除后算加减，同一优先级，从左向右顺序执行,括号优先级最高。
eg: SELECT last_name, salary, salary + 300 FROM employees;

eg: SELECT last_name, salary, 12*salary+100 FROM employees;

eg: SELECT last_name, salary, 12*(salary+100) FROM employees;
```

#### 3.4 空值
* 空值是无效的，未指定的，未知的或不可预知的值。
* 空值不是空格或者0。
```
SELECT last_name, job_id, salary, commission_pct FROM employees;
```
空值在数学运算中的使用
```
包含空值的数学表达式的值都为空值

eg: SELECT last_name, 12*salary*commission_pct FROM employees;
```
使用函数解决空值参与运算的问题
```
如果commission_pct为NULL那么转换成0进行运算。
eg: SELECT last_name, 12*salary*IFNULL(commission_pct,0) FROM employees;
```

#### 3.5 列的别名
* 重命名一个列
* 便于计算
使用别名：
```
SELECT last_name AS name, commission_pct comm,salary "DS & SD 423" FROM employees;

方式一: 使用关键字AS（as）
方式二: 直接紧跟着列名后起名字
方式三: 使用双引号"特殊列名"包裹住,这种方式定义的列名可以包含空格以及其他特殊字符

```

#### 3.6 去除重复行
```
有时候我们查询数据的时候会出现很多重复的数据：
eg: SELECT department_id FROM employees;

可以使用关键字DISTINCT修饰要查询的列：
eg:SELECT DISTINCT department_id FROM employees;

```
#### 3.7 条件查询(过滤和排序)
* 条件查询使用WHERE关键字将不满足条件的数据过滤掉,WHERE 子句紧随FROM子句之后.

```
在WHERE 子句中可以使用如下运算符和关键字

=、!=[<>]、<、<=、>、>=
BETWEEN…AND
IN(set)
IS NULL
AND
OR
NOT
```
```
-- 查询名字(LAST_NAME)是King的员工
SELECT * FROM employees WHERE LAST_NAME='King';

-- 查询名字(LAST_NAME)是King并且FIRST_NAME是Steven的员工。
SELECT * FROM employees WHERE LAST_NAME='King' AND FIRST_NAME="Steven";

-- 查询员工编号（employee_id）为100,101,102号的员工
SELECT * FROM employees WHERE EMPLOYEE_ID IN (100,101,102);

-- 查询员工编号（employee_id）不为100,101,102号的员工
SELECT * FROM employees WHERE EMPLOYEE_ID NOT IN (100,101,102);

--查询COMMISSION_PCT为空或者不为空的记录

SELECT * FROM employees WHERE COMMISSION_PCT IS NOT NULL;
-- 不为NULL的另一种方式
SELECT * FROM employees WHERE NOT COMMISSION_PCT IS NULL;

--查询工资大于9000小于10000的员工信息
SELECT * FROM employees WHERE SALARY>9000 AND SALARY<10000;

-- 查询公司大于等于9000小于等于10000的员工信息 BETWEEN…AND 带边界
SELECT * FROM employees WHERE SALARY BETWEEN 9000 AND 10000;

-- 查询名字不是King的员工信息
SELECT * FROM employees WHERE LAST_NAME !="KING";
SELECT * FROM employees WHERE LAST_NAME <>"KING";
SELECT * FROM employees WHERE  NOT LAST_NAME="KING";

```
* 使用 ORDER BY 子句进行排序
```
– ASC（ascend） : 升序[默认升序]
– DESC（descend） : 降序

ORDER BY 子句放在SELECT语句的结尾
```
```

SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date DESC;

-- 按照别名进行排序
SELECT employee_id, last_name, salary*12 annsal
FROM employees
ORDER BY annsal;

-- 多列排序
SELECT employee_id, last_name, salary*12 as annsal
FROM employees
ORDER BY annsal DESC,last_name ASC;
```

#### 3.8 关联查询(多表连查)

从多个表中获取数据（查询员工编号,员工名称,所在部门信息）
```
SELECT EMPLOYEE_ID,LAST_NAME,department_name 
FROM employees,departments 
WHERE employees.DEPARTMENT_ID= departments.department_id;
```
如果在进行关联查询时，没有添加关联条件，那么会发生笛卡尔积的问题。
```
笛卡尔乘积是指在数学中，两个集合X和Y的笛卡尓积（Cartesian product），又称直积，表示为X×Y，
第一个对象是X的成员而第二个对象是Y的所有可能有序对的其中一个成员
```
笛卡尔积产生的原因:
```
1. 省略连接条件
2. 连接条件无效
3. 所有表中的所有行互相连接
```
怎么避免笛卡尔积:
```
为了避免笛卡尔集， 可以在 WHERE 加入有效的连接条件
```
1. 等值连接(一)
```
SELECT employees.employee_id, employees.last_name,
	   employees.department_id, departments.department_id,
	   departments.location_id
FROM   employees, departments
WHERE  employees.department_id = departments.department_id;
```
2. 多个连接条件与AND操作符
```
SELECT employees.employee_id, employees.last_name,
	   employees.department_id, departments.department_id,
	   departments.location_id
FROM   employees, departments
WHERE  employees.department_id = departments.department_id AND employees.salary>12000;
```
3. 使用表的别名区分重复的列名
```
SELECT e.employee_id, e.last_name,
	   e.department_id, d.department_id,
	   d.location_id
FROM   employees e, departments d
WHERE  e.department_id = d.department_id;
```
4. 多表联查
```
-- 查询出公司员工的 last_name, department_name, city

SELECT e.LAST_NAME,d.department_name,l.city 
FROM employees e,departments d,locations l 
WHERE e.department_id=d.department_id AND d.location_id=l.location_id;
```
5. 非等值连接
```
-- EMPLOYEES表中的列工资应在JOB_GRADES表中的最高工资与最低工资之间的员工名字,工资以及工资级别信息.
SELECT e.last_name, e.salary, j.grade_level
FROM employees e, job_grades j
WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;

```
6. 自连接
```
-- 查询出 last_name 为 'Chen' 的员工的 manager 的信息

SELECT manager.EMPLOYEE_ID,manager.LAST_NAME 
FROM employees work,employees manager 
WHERE work.MANAGER_ID=manager.EMPLOYEE_ID AND work.LAST_NAME="CHen";
```
7. 左外连接 [left]JOIN..ON..语法
```
不止将等值连接的数据查询出来，还会将左侧不符合条件的数据查询出来(不满足等值的数据)

SELECT e.employee_id,e.LAST_NAME,e.DEPARTMENT_ID, d.department_name,d.DEPARTMENT_ID 
FROM employees e LEFT JOIN departments d ON e.department_id=d.department_id;
```
8. 右外连接 [right]JOIN..ON..语法
```
不止将等值连接的数据查询出来，还会将右侧不符合条件的数据查询出来(不满足等值的数据)

SELECT e.employee_id,e.LAST_NAME,e.DEPARTMENT_ID, d.department_name,d.DEPARTMENT_ID 
FROM employees e right JOIN departments d ON e.department_id=d.department_id;
```

9. 等值连接(二) [INNER]JOIN..ON..语法
```
SELECT e.employee_id,e.LAST_NAME,e.DEPARTMENT_ID, d.department_name,d.DEPARTMENT_ID 
FROM employees e INNER JOIN departments d ON e.department_id=d.department_id;
```

---
#### 3.9 模糊查询
模糊查询概念:
<font color="red">
系统允许被搜索信息和搜索提问之间存在一定的差异，这种差异就是模糊在搜索中的含义(根据条件将部分匹配出来的结果展示出来)</font>

使用模糊查询的关键字是LIKE：
```
通匹配:
_：表示任意一个字母
%：表示任意0个或多个字母
```
```
-- 查询LAST_NAME名字中带ab的员工信息
SELECT * FROM employees WHERE LAST_NAME LIKE '%ab%';

-- 查询以bel结尾的员工并且前面只有一个单词
SELECT * FROM employees WHERE LAST_NAME LIKE '_bel';
```
---

#### 3.10 聚合函数
```
聚合函数作用于一组数据，并对一组数据返回一个值，也被称为组函数，或者分组函数
```
常用的组函数:
```
1. COUNT()：统计指定不为NULL的列的总行数
2. MAX()：计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算
3. MIN()：计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算
4. SUM()：计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0
5. AVG()：计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0
```
```
-- 可以对数值型数据使用 AVG、SUM、MIN、MAX 函数
SELECT AVG(salary), MAX(salary), MIN(salary), SUM(salary) 
FROM employees 
WHERE job_id LIKE '%REP%';

-- 可以对任何类型使用 MIN 和 MAX 函数
SELECT MIN(hire_date), MAX(hire_date) FROM employees;

-- COUNT(*) 返回表中记录总数,适用于任意数据类型
SELECT COUNT(*) FROM employees WHERE department_id = 50;

COUNT(expr) 返回expr不为空的记录总数,如果要查询的数据数据为空，则不被记录
SELECT COUNT(commission_pct) FROM employees WHERE department_id = 50;
```
组函数会忽略空值,不将NULL值的列加入运算
```
可以看一下这例子,从这个例子中就会发现组函数，值为NULL的不加入运算
Select avg(commission_pct),sum(commission_pct)/107, sum(commission_pct)/count(commission_pct)
From employees;
```
---

#### 3.11 分组查询
```
分组查询使用 GROUP BY 进行分组
```
1. 求出employees表中各部门的平均工资？
```
SELECT DEPARTMENT_ID,AVG(SALARY) FROM employees GROUP BY DEPARTMENT_ID;
```

2. 求每个部门的部门编号,以及每个部门工资大于8000的人数
```
SELECT DEPARTMENT_ID,count(*) FROM employees WHERE salary>8000 GROUP BY DEPARTMENT_ID;
```
2. 包含在 GROUP BY 子句中的列不必包含在SELECT列表中。
```
SELECT AVG(salary) FROM employees GROUP BY department_id;
```
3. <font color="red">在SELECT列表中所有未包含在组函数中的列都应该包含在GROUP BY子句中。</font>
4. 使用多个列分组
```
-- 通过部门和工种进行分组查询某个部门的某个工种的工人的总工资
SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY department_id, job_id ;
```
5. 非法使用组函数
```
所有包含于SELECT 列表中，而未包含于组函数中的列都必须包含于 GROUP BY 子句中。查询出来的数据是错误的。

SELECT department_id, COUNT(last_name) FROM employees; 

不能在 WHERE 子句中使用组函数,如果组函数作为过滤条件,那么使用HAVING 替换WHERE进行过滤。

SELECT DEPARTMENT_ID, AVG(salary) 
FROM employees 
GROUP BY DEPARTMENT_ID HAVING AVG(SALARY)>8000
```
注：having与where的区别：
```
1. HAVING 是在分组后对数据进行过滤,where是在分组前对数据进行过滤
2. HAVING 后面可以使用分组函数(统计函数)
3. WHERE 后面不可以使用分组函数。
4. WHERE 是对分组前记录的条件，如果某行记录没有满足WHERE子句的条件，那么这行记录不会参加分组
5. 而HAVING是对分组后数据的约束
```
6. 过滤分组HAVING子句
```
1. 行已经被分组。
2. 使用了组函数。
3. 满足HAVING 子句中条件的分组将被显示。

eg：部门最高工资 比 10000 高的部门
SELECT DEPARTMENT_ID,MAX(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID HAVING MAX(SALARY) > 10000;

```

---

#### 3.12 分页查询 LIMIT
LIMIT是MySQL内置函数，其作用是用于限制查询结果的条数:
```
LIMIT语法格式: LIMIT[位置偏移量,行数]

第1个参数是可选参数:MySQL查询分析器要从哪一行开始显示，索引值从0开始，即第一条记录位置偏移量
是0，第二条记录的位置偏移量是1,依此类推...

第2个参数为"行数"即指示返回的记录条数
```
分页换算:
```
SELECT * FROM employees LIMIT 0,5;  --第1页
SELECT * FROM employees LIMIT 5,5;  --第2页
SELECT * FROM employees LIMIT 10,5; --第3页

当前页pageNo     每页显示的条数pageSize            offset 
   1                      5                         0
   2                      5                         5
   3                      5                         10
   4                      5                         15
   ……
   offset=(pageNo-1)*pageSize
```
---

#### 3.13 子查询
```
1. 子查询帮助我们解决的问题
2. 定义子查询
3. 列出子查询类型
4. 书写单行子查询和多行子查询
```

谁的工资比Able的高?
```
直观想法:
第一步: 查出Abel的工资
SELECT SALARY FROM employees WHERE LAST_NAME = "Abel";
第二步: 列出工资大于Abel工资的员工列表
SELECT LAST_NAME,FIRST_NAME,SALARY FROM employees WHERE SALARY > 11000;
```
```
子查询
SELECT LAST_NAME,FIRST_NAME,SALARY FROM employees 
WHERE SALARY >(SELECT  SALARY FROM employees WHERE LAST_NAME = "Abel")
```
注意事项:
* 子查询要包含在括号内
* 将子查询放在比较条件的右侧
* 单行操作符对应单行查查询，多行操作符对应多行子查询

<font color="red">单行子查询</font>
```
1. 只返回一行
2. 使用单行比较运算符 =,>,<,>=,<=,<>[!=]
```
```
-- 返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id 和工资.

SELECT e.LAST_NAME,e.JOB_ID,e.SALARY FROM employees e 
WHERE e.JOB_ID=(SELECT JOB_ID FROM employees WHERE EMPLOYEE_ID=140) 
AND e.SALARY>(SELECT SALARY FROM employees WHERE EMPLOYEE_ID=143)

```
<font color="red">在子查询中使用组函数</font>
```
-- 返回公司工资最少的员工的last_name,job_id和salary
SELECT LAST_NAME,JOB_ID,SALARY FROM employees WHERE SALARY=(SELECT MIN(SALARY) FROM employees);
```
<font color="red">在子查询中的HAVING子句</font>
```
-- 查询最低工资大于50号部门最低工资的部门id和其最低工资

SELECT department_id,MIN(SALARY) 
FROM employees
GROUP BY department_id 
HAVING MIN(SALARY) > (SELECT MIN(SALARY) FROM employees WHERE DEPARTMENT_ID=50);
```
<font color="red">子查询中的空值问题</font>
```
SELECT last_name, job_id
FROM employees
WHERE job_id =(SELECT job_id FROM employees WHERE last_name = 'Haas');

不返回任何行
```

<font color="red">多行子查询</font>
* 返回多行
* 使用多行比较操作符

操作符 | 含义
---|---
IN  | 等于列表中的任意一个(重点)
ANY | 和子查询返回的某一个值比较
ALL | 和子查询返回的所有值比较

IN 操作符使用:
```
-- 查询employee_id 为 110，111，112，113的员工信息

SELECT * FROM employees WHERE employee_id IN(110,111,112,113);
```
---
ALL操作符简介:
```
ALL运算符是一个逻辑运算符，它将单个值与子查询返回的单列值集进行比较,ALL运算符必须以比较运算符
开头，例如：>，>=，<，<=，<>，=，后跟子查询。某些数据库系统(如Oracle)允许使用文字值列表而不是
子查询。
```

条件 | 描述
---|---
c >  ALL(…) | c列中的值必须大于集合中的最大值
c >= ALL(…) | c列中的值必须大于或等于集合中的最大值
c <  ALL(…) | c列中的值必须小于集合中的最小值
c <= ALL(…) | c列中的值必须小于或等于集合中的最小值
c <> ALL(…) | c列中的值不得等于集合中的任何值
c =  ALL(…) | c列中的值必须等于集合中的任何值

```
-- 返回其它部门中比job_id为"IT_PROG"部门所有工资都低的员工的员工号、姓名、job_id 以及salary

SELECT employee_id,LAST_NAME,job_id,salary 
FROM employees 
WHERE SALARY < ALL 
(select salary 
from EMPLOYEES 
WHERE job_id="IT_PROG") 
AND job_id !="IT_PROG";
```
---

ANY操作符简介:
```
ANY运算符是一个逻辑运算符，它将值与子查询返回的一组值进行比较。 
ANY运算符必须以比较运算符：>，>=，<，<=，=，<>开头，后跟子查询.
```
条件 | 描述
---|---
c = ANY (…)  | c列中的值必须与集合中的一个或多个值匹配
c != ANY (…) | c列中的值不能与集合中的一个或多个值匹配
c > ANY (…)  | c列中的值必须大于集合中的最小值
c < ANY (…)  | c列中的值必须小于集合中的最大值
c >= ANY (…) | c列中的值必须大于或等于集合中的最小值
c <= ANY (…) | c列中的值必须小于或等于集合中的最大值
```
-- 返回其它部门中比job_id为"IT_PROG"部门任一工资低的员工的员工号、姓名、job_id 以及salary

SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary < ANY
(SELECT salary
FROM employees
WHERE job_id = 'IT_PROG')
AND job_id <> 'IT_PROG';
```

---
## 第五章 MYSQL数据库的管理


### 第1节 创建MYSQL用户
```
CREATE USER 'reader'@'%' IDENTIFIED BY '111111';
```
* reader: 需要创建的用户名:自定义
* %: IP地址,如果指定了IP地址,那么只有指定的IP机器可以访问,如果指定的是%那么外面所有机器都可以访问
* 111111:密码--自定义的

```
连接MYSQL服务器命令
mysql -h[host] -P[port] -u[user] -p[password]

eg： mysql -h 192.168.1.1 -P3306 -uroot -proot
```
---

### 第2节 用户授权
```
GRANT privileges ON database.table TO 'username'@'%'
```
* privileges分给用户的权限选项，可以是[select ,delete ,update ,insert ]比如可以给用户查询的权限SELECT
* database:数据库名称
* table:表名称
* username:用户名称,要给那个用户分配权限
* %:任意一台机器

简单示例如下:
```
给用户分配所有库的所有表的所有权限 ALL(所有权限)，. 所有库的所有表 
 - GRANT ALL ON *.* TO 'username'@'%'; 
 
给用户分配database库下所有表的所有权限 ALL(所有权限)，database.* database库的所有表 
 - GRANT ALL ON database.* TO 'username'@'%'; 
 
将新增的reader用户分配查询权限 
 - GRANT SELECT ON *.* TO 'reader'@'%';  
```
---

```
刷新系统权限表，一般在给用户分配好权限或者进行权限设置后进行操作
 - flush privileges

修改用户密码，要在当前用户下，修改当前密码，不是在root权限下，修改别的用户密码
 - SET PASSWORD = PASSWORD("newpassword");
 
撤销用户的SELECT权限
 - REVOKE SELECT ON *.* FROM 'reader'@'%';
```
---
```
删除用户，用户名最好是用单引号，不用单引号有可能会报错
 - drop user '用户名'@'%';
```
---

### 第3节 设置其他主机访问此服务器
```
第一步: 修改mysql库user表中host字段,更新host字段为%以后其他任意一台主机都可以访问,也可以指定某一台机器
    update user set host='%' where user='root';
第二步: 刷新权限使其生效 flush  privileges
```
---
## 第六章 MYSQL数据库的编码集修改
### 第1节 查询数据库编写
```
show variables like 'character%';
```
### 第2节 修改mysql数据库编码
```
[mysql]
default-character-set=utf8

[mysqld]
character_set_server=utf8 
collation_server=utf8_general_ci
```
---
## 第七章 mysqldump工具使用

### 第1节 导出指定数据库表
```
mysqldump -u root -p xxx [导出的数据库名称] > [导出后的名字自定义].sql
```
### 第2节 将导出的数据恢复回数据库
```
mysql -uroot -proot music_ < d:/music_.sql
```
---



