---
title: JDK8新特性
categories: javase
tags: ["jdk8"]
---

```
Author: 夜泊
Email : hd1611756908@163.com
QQ    : 1611756908
web   : https://ukoko.gitee.io
sign  : 每走一步,都是进步
```

> JDK发布时间

```xml
JDK8是2014年3月发行版本
```

> JDK8新特性

```java
1. 日期时间API
2. Lambda表达式
3. 默认方法
4. 函数式接口
5. 方法引用
6. Stream API
```

## 第一章 日期和时间

### 第1节 传统的日期时间的缺陷

```
1. 传统的时间处理API设计很差,Java的日期/时间类的定义并不一致,在java.util和java.sql的包中都有日期类
2. 用于格式化和解析的类却在java.text包中定义
3. java.util.Date同时包含日期和时间,而java.sql.Date仅包含日期,这两个类还设计成相同的名字
5. 时区处理麻烦,传统的日期/时间API没有时区支持,所以又引入了java.util.Calendar和java.util.TimeZone类
```

<!--more-->


### 第2节 新的日期时间API的优点

```
JDK8的java.time包下涵盖了所有处理日期、时间、日期/时间、时区等的操作
```

#### 2.1 时间的创建

##### 2.1.1 使用now函数

```java
/**
 * 创建时间
 * 1. 创建当前日期时间(年月日时分秒[毫秒])
 * 2. 创建当前日期(年月日)
 * 3. 创建当前时间(时分秒[毫秒])
 */
 //创建当前日期时间(年月日时分秒[毫秒])
 LocalDateTime localDateTime = LocalDateTime.now();
 //创建当前日期(年月日)
 LocalDate localDate = LocalDate.now();
 //创建当前时间(时分秒[毫秒])
 LocalTime localTime = LocalTime.now();
```

##### 2.2.2 使用of函数

```java
/**
 * 直接构建日期时间
 * of: 直接构建年月日时分秒,并且将毫秒设置成0
 * of: 重载方法
 */
//构建年月日时分秒
LocalDateTime localDateTime1 = LocalDateTime.of(2018, 12, 12, 12, 12, 12);
//构建年月日
LocalDate localDate1 = LocalDate.of(2020, 1, 1);
//构建时分秒
LocalTime localTime1 = LocalTime.of(22, 22, 22);
```

##### 2.2.3 字符串创建

```java
/**
 * 解析字符串,字符串的格式要注意,这是年月日时分秒的默认格式2018-12-30T12:10:59或者2018-12-30T12:10:59.120
 * 如果是其他格式比如2018-12-30 12:10:59需要进行格式转换
 */
//默认格式1
LocalDateTime localDateTime2 = LocalDateTime.parse("2018-12-30T12:10:59");
//默认格式2
LocalDateTime localDateTime3 = LocalDateTime.parse("2018-12-30T12:10:59.120");
//非默认格式1
LocalDateTime localDateTime4 = LocalDateTime.parse("2018-12-30 12:10:59", DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
//非默认格式2
LocalDateTime localDateTime5 = LocalDateTime.parse("2019年12月30日 12时10分59秒",DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH时mm分ss秒"));
```

##### 2.2.4 带时区创建

```java
/**
 * 时区操作
 * 1. 获取其他时区时间
 * 2. 格式化时间
 */
//获取世界标准时间(UTC)
LocalDateTime localDateTime6 = LocalDateTime.now(ZoneId.of("UTC"));
//获取到的时间,比北京时间多8小时
System.out.println("utc: "+localDateTime6);
//东8区(根据utc标准获取其他时区时间很简单,东区就+, 西区就-)
LocalDateTime localDateTime7 = LocalDateTime.now(ZoneId.of("+8"));
System.out.println("GMT8: "+localDateTime7);
```

#### 2.2 时间的获取

* 获取年月日

  ```java
  //获取当前日期年份
  int year = currentDateTime.getYear();
  //获取当前日期的月份枚举值
  Month month = currentDateTime.getMonth();
  //获取当前日期月份的整数值
  int monthValue = currentDateTime.getMonthValue();
  //获取月份中的第几天(取值范围 1~31)
  int dayOfMonth = currentDateTime.getDayOfMonth();
  ```

* 获取时分秒

  ```java
  //获取时(小时)
  int hour = currentDateTime.getHour();
  //获取分钟
  int minute = currentDateTime.getMinute();
  //获取秒
  int second = currentDateTime.getSecond();
  ```

* 获取星期

  ```java
  DayOfWeek dayOfWeek = localDateTime.getDayOfWeek();
  ```

#### 2.3 时间的计算

```java
/**
 * 计算加一年/减一年或者加几年/减几年
 * 计算加一个月/减一个月或者加几个月/减几个月
 * ...
 */
LocalDateTime currentDateTime = LocalDateTime.now();
LocalDateTime localDateTimePlusY1 = currentDateTime.plusYears(10);//加10年
System.out.println("加10年: "+localDateTimePlusY1);
LocalDateTime localDateTimePlusY2 = currentDateTime.minusYears(5);//减5年
System.out.println("减5年: "+localDateTimePlusY2);
LocalDateTime localDateTimePlusM1 = currentDateTime.plusMonths(2);//加2月
System.out.println("加2月: "+localDateTimePlusM1);
LocalDateTime localDateTimePlusM2 = currentDateTime.minusMonths(2);//减2月
System.out.println("减2月: "+localDateTimePlusM2);
//...其他请查找API
```

#### 2.4 时间的修改

```java
 /**
  * 修改日期和时间
  * 可以对当前的日期时间进行修改
  * withYear        : 设置年
  * withMonth       : 设置月
  * withDayOfMonth  : 设置日
  * withHour        : 设置小时
  * withMinute      : 设置分钟
  * withSecond      : 设置秒
  */
 LocalDateTime ldt = currentDateTime.withYear(2018).withMonth(12).withDayOfMonth(30).withHour(12).withMinute(10).withSecond(59);
```

#### 2.5 时间的转换

##### 2.5.1 时间转字符串

```java
String llts = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").format(LocalDateTime.now());
```

##### 2.5.2 时间转时间戳

```java
long milli = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli();
```

## 第二章 Lambda表达式/函数式接口

### 第1节 Lambda表达式

#### 1.1 Lambda表达式介绍

```
1. Lambda 表达式也可称为闭包,它是Java8发布的最重要新特性
2. Lambda 允许把函数作为一个方法的参数
3. Lambda 表达式可以使代码变的更加简洁紧凑
```

#### 1.2 Lambda表达式语法

```
(parameters) -> expression
或
(parameters) ->{ statements; }
```

#### 1.3 Lambda表达式的演进

> 对于Java变量我们可以给其赋值(基本数据类型,引用数据类型)

<img src="https://note.youdao.com/yws/api/personal/file/WEBfd76fd1faecfdf26bd604a0a4eac9248?method=download&shareKey=dd6af32637f99f8d5f0defaa70bc69ee">

> 如果想把"一块代码"赋给一个Java变量,应该怎么做呢？

<img src="https://note.youdao.com/yws/api/personal/file/WEBea77f7addbe5ba4d5e8f64fe90d9dfca?method=download&shareKey=73e2dbe02eb7cca0615e4b7aed77e344">

> 在Java 8之前,这个是做不到的,但是Java8问世之后,利用Lambda特性,就可以做到了

<img src="https://note.youdao.com/yws/api/personal/file/WEB2db9bf18a1c3f14d2baf9c6e02d88453?method=download&shareKey=6cd6f1e2559c27ee1f2bbaa69cf77b9a">

> 上面的写法并不是一个很简洁的写法,为了让这个赋值更加优雅,我们可以移除一些多余的声明

<img src="https://note.youdao.com/yws/api/personal/file/WEBc76d3456f05cc3b78423cb58fb208e0c?method=download&shareKey=cdc334a630013af3f092552bb779405a">

> 这样,我们就成功的非常优雅的把"一块代码"赋给了一个变量,而"这块代码",或者说"这个被赋给一个变量的函数",就是一个Lambda表达式;但是这里仍然有一个问题，就是变量aBlockOfCode的类型应该是什么?

> 在Java8里面,所有的Lambda的类型都是一个接口,而Lambda表达式本身,也就是"那段代码",需要是这个接口的实现.这是我认为理解Lambda的一个关键所在,简而言之就是,Lambda表达式本身就是一个接口的实现.直接这样说可能还是有点让人困扰,我们继续看看例子.我们给上面的aBlockOfCode加上一个类型

<img src="https://note.youdao.com/yws/api/personal/file/WEB3cdb19fad20ddbafd0a26eed79bc1d61?method=download&shareKey=97bbb3f1bcd3e1c5838ef47ea3d04375">

> 这种只有一个接口函数需要被实现的接口类型,我们叫它"函数式接口".为了避免后来的人在这个接口中增加接口函数导致其有多个接口函数需要被实现,变成"非函数接口",我们可以在这个上面加上一个声明@FunctionalInterface,这样别人就无法在里面添加新的接口函数了

<img src="https://note.youdao.com/yws/api/personal/file/WEB172d250168f0eed2770c42148040559d?method=download&shareKey=e73b4640a1e3b93d1c36f802900006f4">

> 这样,我们就得到了一个完整的Lambda表达式声明

<img src="https://note.youdao.com/yws/api/personal/file/WEB4db164cc41e98e7ef160e23dc1191e82?method=download&shareKey=a4f2e8b07f87e15e99455949d76adfd5">

#### 1.4 Lambda表达式的作用

```
最直观的作用就是使得代码变得异常简洁
```

> 对比一下Lambda表达式和传统的Java对同一个接口的实现

<img src="https://note.youdao.com/yws/api/personal/file/WEBf10cb8d7796f3b0c7b334d7433ab513d?method=download&shareKey=c46aec3289a3429dfd94cdf7dc501686">

> 这两种写法本质上是等价的.但是显然,Java8中的写法更加优雅简洁.并且,由于Lambda可以直接赋值给一个变量,我们就可以直接把Lambda作为参数传给函数,而传统的Java必须有明确的接口实现的定义,初始化才行

<img src="https://note.youdao.com/yws/api/personal/file/WEBdbbfc0490ac5e5bd4fd683b312087571?method=download&shareKey=4f1bfe454ac5c3a5781508e0b9c7e885">

> 有些情况下,这个接口实现只需要用到一次.传统的Java7必须要求你定义一个"污染环境"的接口实现MyInterfaceImpl,而相较之下Java8的Lambda,就显得干净很多

#### 1.5 Lambda表达式的简单使用

> 定义函数式接口

```
/**
 * @WebSite    : ukoko.cn
 * @Date       : 2020/11/8 12:11
 * @Description: 自定义函数式接口
 */
@FunctionalInterface
public interface MyLambdaInterface {
    //测试求a+b和
    int testMethodAdd(int a,int b);
}

函数式接口的特点:
1. 要是一个接口
2. 接口内有且只有一个抽象方法
3. 为了防止破坏函数式接口,最好是在接口上面使用@FunctionalInterface注解修饰
```

> 函数式接口的使用

```
/**
 * @WebSite     : ukoko.cn
 * @Date        : 2020/11/8 12:08
 * @Description : Lambda表达式简单使用
 */
public class TestLambda {
    /**
     * 传统方式
     */
    @Test
    public void test01(){
        //匿名内部类(或者是定义一个外部类来实现此接口,然后实例化外部类的方式)
        MyLambdaInterface myLambdaInterface = new MyLambdaInterface() {
            @Override
            public int testMethodAdd(int a, int b) {
                return a+b;
            }
        };
        int methodAdd = myLambdaInterface.testMethodAdd(100, 200);
        System.out.println(methodAdd);
    }
    /**
     * Lambda表达式方式
     */
    @Test
    public void test02(){
        //如果箭头右侧加大括号,不能去掉return关键字
        MyLambdaInterface myLambdaInterface = (a,b)->{return a+b;};
        int methodAdd = myLambdaInterface.testMethodAdd(200, 300);
        System.out.println(methodAdd);
    }
}
```

#### 1.6 Lambda在Java8中的应用

```
Java8中对很多的API进行了修改,加入了函数式编程的语法
```

> 比如集合框架的修改
>
> > 对一个集合中的数据进行排序

* 普通实现

```java
@Test
public void test03(){
    List<String> list = new ArrayList<>();
    list.add("lilei");
    list.add("hanmeimei");
    list.add("jim");
    list.add("tom");
    System.out.println("排序前: "+list);
    //使用Collections工具类进行排序(JDK7)
    Collections.sort(list, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return o1.compareTo(o2);
        }
    });
    System.out.println("排序后: "+list);
}
```

* Lambda实现

```
@Test
public void test04(){
    List<String> list = new ArrayList<>();
    list.add("lilei");
    list.add("hanmeimei");
    list.add("jim");
    list.add("tom");
    System.out.println("排序前: "+list);
    //使用Collections工具类进行排序(如果箭头右侧不加大括号,可以去掉return关键字)
    Collections.sort(list,(s1,s2)->s1.compareTo(s2));
    System.out.println("排序后: "+list);
}
```

### 第2节 函数式接口

#### 2.1 函数式接口介绍

```
函数式接口(Functional Interface)就是一个有且仅有一个抽象方法,但是可以有多个非抽象方法的接口

Java8中新增了很多的函数式接口在java.util.function包下,在其他包下也有函数式接口
```

#### 2.2 常见的函数式接口

* java.util.function包下函数式接口

  ```
  java.lang.Runnable
  java.util.concurrent.Callable
  java.util.Comparator
  java.lang.reflect.InvocationHandler
  ...
  ```

* 其他包常见的函数式接口

  ```
  BiConsumer<T,U>     :代表了一个接受两个输入参数的操作,并且不返回任何结果
  BiFunction<T,U,R>   :代表了一个接受两个输入参数的方法，并且返回一个结果
  Consumer<T>         :代表了接受一个输入参数并且无返回的操作
  DoubleBinaryOperator:代表了作用于两个double值操作符的操作，并且返回了一个double值的结果
  DoubleConsumer      :代表一个接受double值参数的操作,并且不返回结果
  DoubleFunction<R>   :代表接受一个double值参数的方法，并且返回结果
  
  ... 其他请查询API文档
  ```

#### 2.3 函数式接口使用(JDK8提供)

```
public class TestFunctionInterface {
    @Test
    public void test01(){
        /**
         * 接受两个输入参数的操作
         * 没有返回值的函数式接口
         */
        BiConsumer<Integer,Integer> biConsumer = (a,b)-> System.out.println(a+b);
        biConsumer.accept(200,200);
    }

    @Test
    public void test02(){
        /**
         * 接受两个输入参数的操作
         * 并且有返回值
         */
        BiFunction<Integer,Integer,Integer> biFunction = (x,y)-> x+y;
        Integer sum = biFunction.apply(10000, 20000);
        System.out.println("sum= "+sum);
    }
}
```

## 第三章 默认方法/静态方法

### 第1节 默认方法

#### 1.1 默认方法介绍

```
默认方法就是接口可以有实现方法,而且不需要实现类去实现其方法
Java8之前接口里面的方法必须全部都是抽象方法,没有方法体
Java8之后接口中可以定义带有方法体的方法,但是需要在方法前加default关键字进行修饰
```

#### 1.2 默认方法语法

```java
@FunctionalInterface
public interface MyLambdaInterface {
	//默认方法
    default int testDefaultMethod(int a,int b){
        return a+b;
    }
}
```

#### 1.3 默认方法使用

```java
/**
 * 默认方法的调用
 */
@Test
public void test05(){
    //获取实例
    MyLambdaInterface myLambdaInterface = (a,b)->a+b;
    //调用默认方法
    int method1 = myLambdaInterface.testDefaultMethod(100, 300);
    System.out.println(method1);
}
```

> 注意: 如果多个接口定义相同名称和类型的默认方法,因为接口的多实现特性可能会让子类中出现不知道该怎么调用父类接口中默认方法的问题,解决方案有两种,一种是在子类中创建自己的默认方法,覆盖原有的默认方法,第二种是使用父类名.super.方法名进行调用

### 第2节 静态方法

#### 2.1 静态方法介绍

```
静态方法和默认方法差不多,就是将default修饰符修改成statis
```

#### 2.2 静态方法语法

```java
@FunctionalInterface
public interface MyLambdaInterface {
    //静态方法
    static int testStaticMethod(int a,int b){
        return a+b;
    }
}
```

#### 2.3 静态方法使用

```java
@Test
public void test05(){
    //调用静态方法(接口名.方法名)
    int method = MyLambdaInterface.testStaticMethod(1000, 2000);
    System.out.println(method);
}
```

## 第四章 方法引用

### 第1节 方法引用介绍

```
我们在说方法引用前,先说一下Lambda表达式的组成
(a,b)->{return a+b;}
或者
(a,b)-> a+b;

左侧的括号: 函数式接口里面定义的抽象方法的形参列表
箭头: Lambda表达式的操作符,看见这个箭头,就可以认为他是一个Lambda表达式
右侧Lambda体: 函数式接口中,我们实现了函数式接口抽象方法的方法体

对于右侧的Lambda体,可以是一个自定义的表达式(比如上面的a+b)
或者是仅仅调用一个已经存在的方法,这种情况下,可以使用方法引用的方式调用这个已经存在的方法.方法引用的操作符(::)两个冒号.
```

### 第2节 方法引用的语法分类

| 类型           | 语法               | 对应的Lambda表达式                   |
| -------------- | ------------------ | ------------------------------------ |
| 类静态方法引用 | 类名::staticMethod | (args) -> 类名.staticMethod(args)    |
| 实例方法引用   | inst::instMethod   | (args) -> inst.instMethod(args)      |
| 类普通方法引用 | 类名::instMethod   | (inst,args) -> 类名.instMethod(args) |
| 构造方法引用   | 类名::new          | (args) -> new 类名(args)             |

### 第3节 引用的使用

#### 3.1 静态方法引用

* 静态方法介绍

  ```xml
  Lambda表达式中调用某个类的静态方法,且Lambda表达式的参数作为静态方法的入参传入,最后Lambda的方法返回类型要和静态方法的返回类型对上.
  ```

* 代码演示

  ```java
  /**
   * 使用Lambda方式(传统)和Lambda方式(方法引用)方式进行类型转换
   * 静态方法引用
   */
  @Test
  public void test01(){
      //使用Lambda表达式的方式进行String到Integer类型的转换
      Function<String,Integer> function=x->Integer.parseInt(x);
      Integer apply = function.apply("100");
      System.out.println(apply);
  
      //使用方法引用的方式(Lambda的另一种形式)
      Function<String,Integer> fun = Integer::parseInt; //相当于x->Integer.parseInt(x)
      Integer apply1 = fun.apply("300");
      System.out.println(apply1);
  
  }
  ```

#### 3.2 实例方法引用

* 实例方法介绍

  ```
  Lambda表达式中调用某个类的对象的实例方法,且Lambda表达式的参数作为方法的入参传入,最后Lambda的方法返回类型要和实例方法的返回类型对上
  ```

* 代码演示

  ```java
  class A{
      int sum(int a,int b){
          return a+b;
      }
  }
  
  @FunctionalInterface
  interface B{
      int count(int a,int b);
  }
  
  @Test
  public void test02(){
      A a = new A();//实例对象
      B b1 = (x,y)->x+y; //Lambda表达式
      System.out.println(b1.count(1,100));
      B b2 = a::sum; //实例方法引用(实例名称::方法名),相当于(x,y)->x+y
      System.out.println(b2.count(2,200));
  }
  ```

#### 3.3 类普通方法引用

* 类普通方法介绍

  ```
  Lambda表达式中调用Lambda形参中第一个参数的某个实例方法,并且Lambda形参剩余的n-1个参数作为这个实例方法的实参.Lambda的方法返回类型要和对象的实例方法的返回类型对应.
  ```

* 代码演示

  ```java
  @Test
  public void test03(){
      //Lambda表达式
      Function<String,String> fun1 = (x)->x.toUpperCase();
      System.out.println(fun1.apply("abcdefg"));
      //类普通方法引用(类名::方法名),相当于(x)->x.toUpperCase()
      Function<String,String> fun2 = String::toUpperCase;
      System.out.println(fun2.apply("hijklmn"));
  }
  ```

#### 3.4 构造方法引用

* 构造方法介绍

  ```
  Lambda的形参作为某个类的构造方法/方法的实参
  ```

* 代码演示

  ```java
  public class User{
      private Integer userId;
      private String userName;
  
      public static User createUser(Supplier<User> supplier){
          return supplier.get();
      }
  }
  
  @Test
  public void test04(){
      //传统Lambda表达式
      User user1 = User.createUser(()->{return new User();});
      user1.setUserId(1001);
      user1.setUserName("李雷");
      //构造器方法引用
      User user2 = User.createUser(User::new);
      user2.setUserId(2001);
      user2.setUserName("韩梅梅");
      System.out.println(user1);
      System.out.println(user2);
  }
  ```

## 第五章 Stream API

### 第1节 Stream API介绍

#### 1.1 Stream是什么?

```
Stream API是Java8中加入的一套新的API,主要用于处理一组数据的操作
它的处理方式与传统的方式不同,称为"数据流处理"
就是将集合/数组/文件/函数等数据转换成流之后再处理(比如对集合数据进行过滤,排序等操作)
Stream并不是数据结构所以不能保存数据,主要目的在于计算
```

#### 1.2 简述Stream优势

```java
class User{
    //用户ID
    private Integer userId;
    //用户名称
    private String userName;
    //用户年龄
    private Integer age;
    //班级ID
    private Integer classId;
}
//创建集合
List<User> users = new ArrayList<>();
// 生成数据略
/**
 * JDK7,从集合中过滤出年级小于18的用户
 */
for (User user : users) {
    if(user.getAge()<18){//小于18的用户
        System.out.println("JDK7 >>>年纪小于18的用户为: "+user);
    }
}
/**
 * JDK8流操作
 * 1. 现将集合转换成Stream
 * 2. 使用内部提供的方法操作
 * 3. JDK8的流式操作不仅仅有过滤还有排序,限制个数,去重,以及类似于数据的组函数的操作等.
 */
//转换成Stream
Stream<User> userStream = users.stream();
//过滤和排序转换回集合
List<User> userList = userStream.filter(x -> x.getAge() < 18).sorted((x,y)->y.getClassId().compareTo(x.getClassId())).collect(Collectors.toList());
System.out.println("JDK8 >>>年纪小于18的用户为: "+userList);
```

### 第2节 Stream API使用

#### 2.1 生成Stream的方式

* 通过集合生成Stream

  ```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
  Stream<Integer> stream1 = list.stream();
  ```

* 通过数组生成Stream

  ```java
  int[] arr = new int[]{1,2,3,4,5,6,7,8,9,0};
  IntStream stream2 = Arrays.stream(arr);
  ```

* 通过值生成Stream

  ```java
  Stream<Integer> stream3 = Stream.of(100, 200, 150, 210, 105);
  ```

* 通过文件生成Stream

  ```java
  Stream<String> stream4=Files.lines(Paths.get("stream.txt"),Charset.defaultCharset());
  ```

* 通过函数生成Stream

* * iterator

    ```java
    //通过iterator生成5个偶数Stream,iterator生成的流为无限流,所以用limit截断,只生成5个
    Stream<Integer> stream5 = Stream.iterate(0, n -> n + 2).limit(5);
    ```

  * generator

    ```java
    //通过generator生成5个随机数,generator生成的流为无限流,所以用limit截断,只生成5个
    Stream<Double> stream6 = Stream.generate(Math::random).limit(5);
    ```

#### 2.2 Stream的使用

```
流的操作类型主要分为两种:
1. 中间操作
2. 终端操作
```

##### 2.2.1 中间操作

> 一个流可以后面跟随零个或多个中间操作,目的是打开流,做出将要对数据进行某种操作的指示,然后返回一个新流,交给下一个操作使用.这种操作是惰性的,仅仅调用了这个类的方法,并不会进行具体的操作,真正操作需等到终端操作

###### 2.2.1.1 filter筛选

```java
@Test
public void test01(){
    List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //filter筛选(筛选出大于3的)
    List<Integer> collect = stream.filter(x -> x > 3).collect(Collectors.toList());
    System.out.println("原数据:"+integers);
    System.out.println("筛选之后的数据:"+collect);
}
```

###### 2.2.1.2 distinct去除重复元素

```java
@Test
public void test02(){
    List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //distinct去除重复元素
    List<Integer> collect = stream.distinct().collect(Collectors.toList());
    System.out.println("原数据:"+integers);
    System.out.println("去重之后的数据:"+collect);
}
```

###### 2.2.1.3 limit返回指定流个数

```java
@Test
public void test03(){
    List<Integer> integers = Arrays.asList(1,2,2, 3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //limit返回指定流个数
    List<Integer> collect = stream.limit(5).collect(Collectors.toList());
    System.out.println("原数据:"+integers);
    System.out.println("目标数据:"+collect);
}
```

###### 2.2.1.4 skip跳过流中的元素

```java
@Test
public void test04(){
    List<Integer> integers = Arrays.asList(1,2,2,3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //skip跳过流中几个元素
    List<Integer> collect = stream.skip(10).collect(Collectors.toList());
    System.out.println("原数据:"+integers);
    System.out.println("目标数据:"+collect);
}
```

###### 2.2.1.5 map流映射

```java
@Test
public void test05(){
    List<Integer> integers = Arrays.asList(1,2,2,3);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //map流映射(将当前的元素映射成另外一个元素)[将int类型映射成String类型]
    List<String> collect = stream.map(x -> x.toString()).collect(Collectors.toList());
    System.out.println("原数据:"+integers);
    System.out.println("目标数据:"+collect);
}
```

###### 2.2.1.6 flatMap流转换

```java
@Test
public void test06(){
    List<String> strings = Arrays.asList("H e l l o", "Java8");
    //转换成Stream
    Stream<String> stream = strings.stream();
    /**
     * flatMap流转换(将一个流中的每个值都转换为另一个流)
     * 先将集合中的每个字符串都转成转换成数组
     * 在将数组转换成Stream
     */
    List<String> collect = stream.map(x -> x.split(" ")).flatMap(Arrays::stream).collect(Collectors.toList());
    System.out.println("原数据:"+strings);
    System.out.println("目标数据:"+collect);
}
```

###### 2.2.1.7 元素匹配

> 提供了三种匹配方式
>
> > 1. allMatch匹配所有
> > 2. anyMatch匹配其中一个
> > 3. noneMatch全部不匹配

```java
@Test
public void test07(){
    List<Integer> integers = Arrays.asList(1,2,3);
    /**
     * 元素匹配
     * 1. allMatch匹配所有
     * 2. anyMatch匹配其中一个
     * 3. noneMatch全部不匹配
     */
    //匹配Stream中的所有元素是否都大于3,如果大于3返回true,否则返回false
    if(integers.stream().allMatch(x -> x > 3)){
        System.out.println("全部大于3");
    }
    //anyMatch匹配其中一个,有一个成立即可
    if(integers.stream().anyMatch(x -> x > 3)){
        System.out.println("存在大于3的数");
    }
    //noneMatch全部不匹配
    if(integers.stream().noneMatch(x -> x > 3)){
        System.out.println("全部小于等于3");
    }
}
```

##### 2.2.2 终端操作

###### 2.2.2.1 统计流中元素个数

```java
@Test
public void test01(){
    List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //统计流中元素个数
    long count = stream.count();
    System.out.println("count:"+count);
}
```

###### 2.2.2.2 查找

* findFirst查找第一个

* findAny随机查找一个

  ```java
  @Test
  public void test02(){
      List<Integer> integers = Arrays.asList(100,1, 2, 3, 4, 5, 6, 7, 8, 8, 7, 6, 65, 5);
      //转换成Stream
      Stream<Integer> stream = integers.stream();
      //findFirst查找第一个/findAny随机查找一个(如果是串行流,那么和findFirst结果相同,如果是并行流就不一定了)
      //Optional<Integer> first = stream.findFirst();
      Optional<Integer> any = stream.findAny();
      //System.out.println("first:"+first.get());
      System.out.println("any:"+any.get());
  }
  ```

###### 2.2.2.3 reduce将流中的元素组合起来

```java
@Test
public void test03(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    /**
     * 将intergers集合中的数据求和(组合起来)
     * reduce方法接受两个入参
     * 1. 代表初始化值
     * 2. BinaryOperator将两个元素组合起来生成一个新元素
     */
    //Integer reduce = stream.reduce(0, (x, y) -> x + y);
    Integer reduce = stream.reduce(0, Integer::sum);
    System.out.println(reduce);
}
```

###### 2.2.2.4 获取流中最小最大值

```java
@Test
public void test04(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //最大值/最小值
    //Optional<Integer> min = stream.min((x, y) -> x.compareTo(y));
    //Optional<Integer> max = stream.max((x, y) -> x.compareTo(y));
    Optional<Integer> max = stream.max(Integer::compareTo);
    //System.out.println(min.get());
    System.out.println(max.get());
}
```

###### 2.2.2.4 求和

```java
@Test
public void test05(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //求和(使用sum方法,如果使用sum方法需要mapToInt转换)
    int sum = stream.mapToInt(x -> x).sum();
    System.out.println(sum);
}
```

###### 2.2.2.5 通过averagingInt求平均值

```java
@Test
public void test06(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5,10);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //求平均值averagingInt,如果流式Double或者Long那么使用AveragingDouble,AveragingLong
    Double collect = stream.collect(Collectors.averagingInt(Integer::intValue));
    System.out.println(collect);
}
```

###### 2.2.2.6 通过summarizingInt同时求总和、平均值、最大值、最小值

```java
@Test
public void test07(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5,10);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    IntSummaryStatistics collect = stream.collect(Collectors.summarizingInt(Integer::intValue));
    //获取平均值
    double average = collect.getAverage();
    //获取最小值
    int min = collect.getMin();
    //获取最大值
    int max = collect.getMax();
    System.out.println(average);
    System.out.println(min);
    System.out.println(max);
}
```

###### 2.2.2.7 通过foreach进行元素遍历

```java
@Test
public void test08(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5,10);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    //stream.forEach(x-> System.out.println(x));
    stream.forEach(System.out::println);
}
```

###### 2.2.2.8 返回集合

```java
@Test
public void test09(){
    List<Integer> integers = Arrays.asList(1,2,3,4,5,10);
    //转换成Stream
    Stream<Integer> stream = integers.stream();
    List<Integer> collect = stream.collect(Collectors.toList());
    //Set<Integer> collect1 = stream.collect(Collectors.toSet());
    System.out.println(collect);
    //System.out.println(collect1);
}
```

###### 2.2.2.9 通过joining拼接流中的元素

```java
@Test
public void test10(){
    List<String> strings = Arrays.asList("Hello","Java8");
    //转换成Stream
    Stream<String> stream = strings.stream();
    String collect = stream.map(s -> s.toString()).collect(Collectors.joining("-"));
    System.out.println(collect);
}
```

###### 2.2.2.10 通过groupingBy进行分组

```java
class User{
    private Integer userId;
    private String userName;
    private int age;
}

@Test
public void test11(){
    List<User> users = Arrays.asList(new User(1001, "李雷", 18), new User(1002, "李雷2", 18), new User(1003, "李雷3", 19), new User(1004, "李雷4", 19));
    //转换成Stream
    Stream<User> stream = users.stream();
    //根据年龄分组
    Map<Integer, List<User>> collect = stream.collect(Collectors.groupingBy(User::getAge));
    System.out.println(collect);
}
```