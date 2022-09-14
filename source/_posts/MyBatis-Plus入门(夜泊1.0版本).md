---
title: MyBatis-Plus笔记
categories: MyBatis-Plus
tags: ["MyBatis-Plus"]
---

```
Author: 夜泊
Email : hd1611756908@163.com
QQ    : 1611756908
web   : https://ukoko.gitee.io
sign  : 每走一步,都是进步
```

> 官网地址

```
https://baomidou.com/
```

> 学习导读: 
>
> 1. 首选要有springboot基础
> 2. 要有mybatis基础

## 第一章 MyBatis-Plus简介

### 第1节  MyBatis-Plus是什么

>  MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生 

<!--more-->

### 第2节 MyBatis-Plus的特性

*  **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响 
* **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
* **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
* **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
* **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
* **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
*  **支持自定义全局通用操作**：支持全局通用方法注入 
*  **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码 
* **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
* **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
* **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
* **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 第二章 MyBatis-Plus 快速入门

### 第1节 添加依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>

springboot版本为<version>3.4.1</version>
```

### 第2节 配置

```
在main方法所在的类上面添加注解扫描mapper层的包
@MapperScan(value = "cn.ukoko.uko_demo.mapper")
```

### 第3节 通用Mapper层接口使用

#### 3.1 创建数据库表

```sql
CREATE TABLE user(
	user_id BIGINT(20) NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT '主键ID',
	user_name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	hire_date DATETIME DEFAULT NULL COMMENT "入职时间"
)
```

#### 3.2 创建mapper层接口

```java
//创建接口,继承mapper层通用接口
public interface UserMapper extends BaseMapper<User> {

}
```

#### 3.3 基本的CRUD

```java
class UserMapperTest extends UkoDemoApplicationTests {
    @Resource
    private UserMapper userMapper;
    /**
     * 添加用户
     * 设置User类主键为自增 @TableId(type = IdType.AUTO)
     */
    @Test
    public void addUser(){
        User user = new User();
        user.setUserName("李雷2");
        user.setHireDate(LocalDateTime.now());
        int row = userMapper.insert(user);
        System.out.println(row);
    }

    /**
     * 查询全部
     */
    @Test
    public void getUsers(){
        //查询全部
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }

    /**
     * 分页查询
     * 详见: https://baomidou.com/guide/page.html
     */
    @Test
    public void getUserPage(){
        IPage<User> page = new Page<>(2,2);
        IPage<User> page1 = userMapper.selectPage(page, null);
        List<User> records = page1.getRecords();
        System.out.println(records);
    }

    /**
     * 根据userId查询
     */
    @Test
    public void getUserByUsrId(){
        /**
         * 如果数据库主键的名称不为id那么需要使用注解进行设置
         * 详解： https://baomidou.com/guide/annotation.html#tableid
         * 通过注解指定主键的名称对应Java实体类的字段
         */
        User user = userMapper.selectById(5);
        System.out.println(user);
    }
    /**
     * 删除操作
     */
    @Test
    public void deleteUser(){
        int row = userMapper.deleteById(6);
        System.out.println(row);
    }

    /**
     * 修改
     */
    @Test
    public void updateUser(){
        User user = new User();
        user.setUserId(3);
        user.setUserName("韩梅梅3");
        //根据ID更新
        int row = userMapper.updateById(user);
        System.out.println(row);
    }
    /**
     * 根据其他条件查询
     */
    @Test
    public void getUsersByOther(){
        /**
         * 构建查询条件的类
         * 详见: https://baomidou.com/guide/wrapper.html#abstractwrapper
         */
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        User user = new User();
        user.setUserName("韩梅梅3");
        queryWrapper.setEntity(user);
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);
    }
}
```

### 第4节 创建Service层接口

#### 4.1 创建Service层接口/Service层实现类

##### 4.1.1 Service层接口

```java
public interface UserService extends IService<User> {

}
```

##### 4.1.2 Service层实现类

```java
@Service
@Transactional
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {

}
```

#### 4.2 基本的CRUD

```java
/**
 * 通用Service层接口
 * 详见: https://baomidou.com/guide/crud-interface.html#service-crud-%E6%8E%A5%E5%8F%A3
 */
class UserServiceTest extends UkoDemoApplicationTests {
    @Autowired
    private UserService userService;


    /**
     * 查询全部
     */
    @Test
    public void getUsers(){
        List<User> users = userService.list();
        System.out.println(users);
    }
    /**
     * 查询分页
     */
    @Test
    public void getUserPage(){
        IPage<User> page = new Page<>(2,2);
        IPage<User> iPage = userService.page(page);
        List<User> records = iPage.getRecords();
        records.forEach(System.out::println);
    }
    /**
     * 通过ID查询
     */
    @Test
    public void getUser(){
        User user = userService.getById(3);
        System.out.println(user);
    }
    /**
     * 添加
     */
    @Test
    public void addUser(){
        User user = new User();
        user.setUserName("Tom2");
        user.setHireDate(LocalDateTime.now());
        boolean save = userService.save(user);
        System.out.println(save);
    }

    /**
     * 更新
     */
    @Test
    public void upadteUser(){
        User user = new User();
        user.setUserId(3L);
        user.setUserName("Tom3");
        user.setHireDate(LocalDateTime.now());
        userService.updateById(user);
    }

    /**
     * 存在主键就更新否则添加
     */
    @Test
    public void saveAndUpadteUser(){
        User user = new User();
        //user.setUserId(3L);
        user.setUserName("Tom444");
        user.setHireDate(LocalDateTime.now());
        userService.saveOrUpdate(user);
    }
    /**
     * 删除
     */
    @Test
    public void deleteUser(){
        boolean remove = userService.removeById(4);
        System.out.println(remove);
    }
}
```

### 第5节 MyBatis-Plus支持原生

```
可以像使用原生mybatis那么进行编写代码,代码操作规范不变
```



