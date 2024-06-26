---
title: 快速入门RabbitMQ
categories: RabbitMQ
tags: ["消息队列","rabbitmq","消息队列"]
---

```
作者: 夜泊1990
企鹅: 1611756908
鹅群: 948233848
邮箱: hd1611756908@163.com
博客: https://hd1611756908.github.io/
B 站: https://space.bilibili.com/514155929/
```
---

## 第一章 RabbitMQ介绍

### 第1节 MQ是什么
```
1. 消息队列(Message Queue),又叫做消息中间件
2. 用高效可靠的消息传递机制进行与平台无关的数据交流,并基于数据通信来进行分布式系统的集成
3. 通过提供消息传递和消息队列模型,可以在分布式环境下扩展进程的通信
4. MQ 是用来解决通信的问题,主要用来帮我们存储和转发消息
```
---

<!--more-->

### 第2节 MQ 的主要特点
```
1. 是一个独立运行的服务.生产者发送消息,消费者接收消费,需要先跟服务器建立连接
2. 采用队列作为数据结构,有先进先出的特点
3. 具有发布/订阅的模型,消费者可以获取自己需要的消息
```
---
### 第3节 RabbitMQ介绍

* 官网地址

```
https://www.rabbitmq.com/
```

```
官网介绍: RabbitMQ is the most widely deployed open source message broker
中文解释: RabbitMQ 是部署最广泛的开源消息队列代理服务器

RabbitMQ是一个开源的AMQP(高级消息队列协议)实现,服务器端用Erlang语言编写,支持多种客户端,如:Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等
```
---
## 第二章 RabbitMQ安装(Docker)

### 第1节 安装步骤
```
1. 首先就是查询镜像,我们要用带有management版本的，此版本是带有管理界面的
    
    docker search rabbitmq:management

2. 拉取镜像

    docker pull rabbitmq:management

3. 查看已下载的rabbitmq镜像

    docker images

4. 运行RabbitMQ服务器

    docker run --name rabbitmq-server -d -p 15672:15672 -p 5672:5672 rabbitmq:management
    
    4.1 –name指定了容器名称
    4.2 -d 指定容器以后台守护进程方式运行
    4.3 -p指定容器内部端口号与宿主机之间的映射，rabbitmq默认要使用15672为其web端界面访问时端口，5672为数据通信端口

5. 执行docker ps 命令查看当前运行服务器列表

6. 查看启动的RabbitMQ服务器日志命令
    
    docker logs -f 容器ID
    
    从打印的日志中可以发现这几句日志信息
    Creating user ‘guest’ // 默认创建了用户名 guest
    Setting user tags for user ‘guest’ to [administrator] // 添加了administrator角色
    Setting permissions for ‘guest’ in ‘/‘ to ‘.‘, ‘.‘, ‘.*’ // 密码为 guest

7. 访问RabbitMQ的web管理页面

    在浏览器中输入: http://192.168.1.129:15672
    
    192.168.1.129是我的主机IP,换成个人的IP地址即可
    输入默认创建的用户名和密码 guest guest
```

<img src="https://note.youdao.com/yws/api/personal/file/D076E449167648528F050D3073B89206?method=download&shareKey=024152ae70e2306af337b15040844d3b" alt="RabbitMQ-success" style="zoom:80%;" />

---
### 第2节 用户管理
```
给RabbitMQ服务器添加新用户并且赋予权限
    
    1. 进入容器
        docker exec -i -t 0e7e57214cfb bin/bash 通过容器ID进入容器内部
        
    2. 添加用户名和密码都是root的用户
        rabbitmqctl add_user root root
    
    3. 赋予root用户所有权限
        rabbitmqctl set_permissions -p / root ".*" ".*" ".*"
    
    4. 赋予root用户administrator角色
        rabbitmqctl set_user_tags root administrator
    
    5. 查看所有用户列表,即可看到我们添加的root
        rabbitmqctl list_users
    
    6. 回到我们的web管理页面,将原有的用户注销,使用新创建的root用户登录测试
```
---
## 第三章 RabbitMQ架构及通讯范式

### 第1节 RabbitMQ的整体架构图

|                             架构                             |
| :----------------------------------------------------------: |
| <img src="https://note.youdao.com/yws/api/personal/file/8863E25C1B284CC1A8970B4B57E836E6?method=download&shareKey=e86315ae9c7142836439b405eeb18ba7" alt="RabbitMQ" style="zoom:80%;" /> |

---

* Producer

```
消息发布者,主要用来进行消息发布
```

* Exchange

```
交换器,消息发布者将消息发送给交换器,交换器在通过路由发送到队列中
```

* Queue

```
保存消息
```

* Customer

```
消息消费者
```

> 一个消息(message)从开始到结束的过程

```
消息(message)被发布者(Producer)发送给交换器(exchange)然后交换机将收到的消息根据路由规则分发给绑定的队列(queue)最后将消息投递给订阅了此队列的消费者(consumer),或者消费者按照需求自行获取
```

---
## 第四章 快速入门(RabbitMQ-HelloWorld)

```
RabbitMQ快速入门,使用Java语言来向RabbitMQ服务器发送和消费消息
```
### 第1节 原理图

|               HelloWorld               |
| :------------------------------------: |
| ![python-one](https://note.youdao.com/yws/api/personal/file/73FEFD26A1A9415DAFA7FAA35666DB2E?method=download&shareKey=a214ae16b858ce24c5bc0bf89125cff0) |

* P: producer  生产者
* C: consumer  消费者
* 中间红色的部分是一个队列,在RabbitMQ中代表消息缓冲区

---
### 第2节 客户端依赖

> 在编写Java代码之前首先需要创建Java项目并且添加maven依赖,使用的是5.8.0版本的MQ客户端

```
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>5.8.0</version>
</dependency>

依赖添加完成之后就是编写代码
```
---
### 第3节 消息发布

* 代码

```java
/**
 * 向队列中发送一个消息
 */
public class ProducerMessage {
    //声明一个队列名称
    private static final String QUEUE_NAME="hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);
        //构建一个消息
        String message = "Hello RabbitMQ";
        /*
            向队列中发送上面的message消息
            里面涉及到两个参数
            第2个参数  routingKey : 指定发送队列的名称
            第4个参数  body       : 设置需要发送的消息,byte数组格式
            其它参数会在后面介绍其它功能时详解
         */
        channel.basicPublish("",QUEUE_NAME,null,message.getBytes());
        System.out.println(" [x] Sent '" + message + "'");
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 控制台查看

|                           管理页面                           |
| :----------------------------------------------------------: |
| <img src="https://note.youdao.com/yws/api/personal/file/857866450CCF4C4EB915A1D2520023FF?method=download&shareKey=ab85826f124ccb750b8a0a49ee90389e" alt="RabbitMQ-info1"  /> |
|        ![RabbitMQ-info2](https://note.youdao.com/yws/api/personal/file/8112AB223BD9422EBC756A94F0A84D56?method=download&shareKey=9c582e5c4c58c616463ec1b95cb15107)        |

---
### 第4节 消息消费

* 代码

```java
/**
 * 从队列中获取消息
 */
public class ConsumerMessage {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        /*
            消费队列里面的消息
            参数介绍: 暂时只关注第1、3、4这三个参数,第二个参数在下面的案例中会介绍到
            第1个参数: 设置待消费的队列名称
            第3个参数: 当消息被传递过来时，回调
            第4个参数: 当消费者被意外取消或关闭时回调
         */
        channel.basicConsume(QUEUE_NAME,true,deliverCallback,consumerTag -> {});
    }
}
```

* 控制台查看

```
当消费完成在回到RabbitMQ的web管理页面查看消息,会发队列中的消息已经被消费
```
* 注意
```
在启动代码的时候先启动消费者,后启动提供者,先启动消费者创建队列,后启动发布者向队列中发布数据
当前入门的例子,因为手动创建了具体的队列所以不会出现发送消息到指定队列不存在的问题,但是接下来后面的练习可能会出错.
```
---
## 第五章 工作队列(RabbitMQ-Work)

### 第1节 原理图

![python-two](https://note.youdao.com/yws/api/personal/file/4206F7B415534996A787434BF870F1BF?method=download&shareKey=cde013258e1b91a4d13683ffe7285112)

---
### 第2节 Work(工作队列)介绍

```
HelloWorld入门的例子介绍了我们提供者向队列中发送一个消息,消费者从队列中取出消息消费;接下来我们将要创建一个工作队列,用来在多个消费者(consumer)之间分发任务,然后观察,我们分发的任务在多个工作者之间是怎么进行消费的
```
* 工作队列(任务队列)
```
例如消息提供者将消息发送到任务队列中,根据上面图示,任务队列会被多个消费者(consumer)连接,这时候如果我们向任务队列中发送多个任务,这些任务会被所有的消费者(consumer)共享,那么问题来了,这个任务队列是怎么将任务合理的分配给消费者(consumer)进行消费的.
```
---
### 第3节 Work(工作队列)实现

* 需求举例

```
消息发布者: 向队列中发送多条消息(比如我这里发送10条消息)
消息消费者: 创建多个消费者验证,查看每个消费者处理的消息任务数(我这里创建两个消费者)
```

* 消息发布代码

```java
/**
 * 向工作队列中发送消息
 */
public class ProducerWorkMessage {
    //声明一个队列名称
    private static final String QUEUE_NAME="hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);
        //构建10条消息并且发送到队列
        for (int i = 0; i < 10; i++) {
            String message = "Hello RabbitMQ i="+i;
            channel.basicPublish("",QUEUE_NAME,null,message.getBytes());
            System.out.println(" [x] Sent '" + message + "'");
        }
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 消息消费代码(下面是两个消费者)

```java
//消费者1
/**
 * 从队列中获取消息
 */
public class ConsumerWorkMessage1 {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        /*
            消费队列里面的消息
            参数介绍: 暂时只关注第1、3、4这三个参数,第二个参数在下面的案例中会介绍到
            第1个参数: 设置待消费的队列名称
            第3个参数: 当消息被传递过来时，回调
            第4个参数: 当消费者被意外取消或关闭时回调
         */
        channel.basicConsume(QUEUE_NAME,true,deliverCallback,consumerTag -> {});
    }
}


//消费者2
/**
 * 从队列中获取消息
 */
public class ConsumerWorkMessage2 {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);
        
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        /*
            消费队列里面的消息
            参数介绍: 暂时只关注第1、3、4这三个参数,第二个参数在下面的案例中会介绍到
            第1个参数: 设置待消费的队列名称
            第3个参数: 当消息被传递过来时，回调
            第4个参数: 当消费者被意外取消或关闭时回调
         */
        channel.basicConsume(QUEUE_NAME,true,deliverCallback,consumerTag -> {});
    }
}
```

* 结果展示

```
上面代码所得的结果显示: 

消息发布者发送的10条消息:
    [x] Sent 'Hello RabbitMQ i=0'
    [x] Sent 'Hello RabbitMQ i=1'
    [x] Sent 'Hello RabbitMQ i=2'
    [x] Sent 'Hello RabbitMQ i=3'
    [x] Sent 'Hello RabbitMQ i=4'
    [x] Sent 'Hello RabbitMQ i=5'
    [x] Sent 'Hello RabbitMQ i=6'
    [x] Sent 'Hello RabbitMQ i=7'
    [x] Sent 'Hello RabbitMQ i=8'
    [x] Sent 'Hello RabbitMQ i=9'

消费者1消费的消息: 1,3,5,7,9
    [x] Received 'Hello RabbitMQ i=1'
    [x] Received 'Hello RabbitMQ i=3'
    [x] Received 'Hello RabbitMQ i=5'
    [x] Received 'Hello RabbitMQ i=7'
    [x] Received 'Hello RabbitMQ i=9'


消费者2消费的消息: 0,2,4,6,8
    [x] Received 'Hello RabbitMQ i=0'
    [x] Received 'Hello RabbitMQ i=2'
    [x] Received 'Hello RabbitMQ i=4'
    [x] Received 'Hello RabbitMQ i=6'
    [x] Received 'Hello RabbitMQ i=8' 
    

```

* 结果分析
```

1. 通过上面的结果可以看出发布者发布消息会平均分配给每一个消费者,采用默认的任务分发机制(轮询)
2. 这种方式有优点当然也有缺点
    2.1 优点: 可以轻易的并行工作,如果我们积压很多任务，我们可以通过增加工作者(consumer)来解决这一问题,使得系统的伸缩性更强
    2.2 缺点: 这种分发机制没有考虑处理处理任务的时间问题(因为他分配任务的时候是一次性分配,并非是一个一个分配),按照轮询的方式将任务等分给了两个消费者,可能某一个消费者性能比较差,累积任务会越来越多,所以一直忙个不停;而另一个消费者性能比较好,处理任务块,可能闲的不行,这就造成了资源浪费


    怎么解决这个问题呢? ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓ 向下看 ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
```

### 第4节 Fair dispatch(公平分发)

#### 4.1 原理图

|                  公平分发                  |
| :----------------------------------------: |
| ![python-eight](https://note.youdao.com/yws/api/personal/file/BF2A1C28FB224A9CB377B696083EBD23?method=download&shareKey=6faeae09766496483f4324d86177b69b) |

---
#### 4.2 公平分发原则

```
上面的轮询机制会造成资源浪费的问题,原因是因为RabbitMQ在分发任务的时候盲目的一次性平均分配任务,它不看消费者是否应答(分发给消费者的任务后,不看消费者是否完成,直接在分配，这样就造成了累积),为了解决这个问题,RabbitMQ提供了一个方法来解决


int prefetchCount = 1;
channel.basicQos(prefetchCount);

这段代码的意义就是来限制RabbitMQ只发不超过1条的消息给同一个消费者,且当消息处理完毕后,有了反馈,才会进行第二次发送.


当然在使用basicQos方法的时候还需要设置两个参数
1、设置autoAck为false channel.basicConsume(QUEUE_NAME,false,deliverCallback,consumerTag -> {})

2、在从MQ获取到消息之后设置 channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);

这两个参数的意义: 
    autoAck: 设置应答模式,默认是自动应答
        - 自动应答:只要消息从队列中获取，无论消费者获取到消息后是否成功消息，都认为是消息已经成功消费
        - 手动应答:消费者从队列中获取消息后,服务器会将该消息标记为不可用状态，等待消费者的反馈,如果消费者一直没有反馈,那么该消息将一直处于不可用状态,打开手动确认还有一个好处就是当消费者不正常死亡(通道关闭,连接关闭或TCP连接丢失),RabbitMQ会将这个消息进行重新加入队列进行排队.当然如果正常消费,RabbitMQ就会正常删除掉他.
        - 当使用公平分发的时候,必须取消自动应答模式,改为手动应答模式.
```

---
#### 4.3 代码实现

* 消息发布代码

```java
/**
 * 向任务队列中发送消息
 */
public class ProducerWorkMessage {
    //声明一个队列名称
    private static final String QUEUE_NAME="hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);

        //限制发送消息给同一个消费者不得超过一条
        int prefetchCount = 1;
        channel.basicQos(prefetchCount);

        //构建10条消息并且发送到队列
        for (int i = 0; i < 10; i++) {
            String message = "Hello RabbitMQ i="+i;
            channel.basicPublish("",QUEUE_NAME,null,message.getBytes());
            System.out.println(" [x] Sent '" + message + "'");
        }
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 消息消费代码

```java
// 消费者1

/**
 * 从队列中获取消息
 */
public class ConsumerWorkMessage1 {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);
        //保证发送给消费者的消息只分发一次
        channel.basicQos(1);
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
            //开启手动消息应答
            channel.basicAck(delivery.getEnvelope().getDeliveryTag(),false);
        };
        /*
            消费队列里面的消息
            参数介绍: 这里只说第2个参数，其他参数前面已经说过
            第2个参数 autoAck : false
         */
        channel.basicConsume(QUEUE_NAME,false,deliverCallback,consumerTag -> {});
    }
}

//消费者2

/**
 * 从队列中获取消息
 */
public class ConsumerWorkMessage2 {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,现在只关注第1个参数,队列名称,后面其他参数会在下面的例子中一个个讲解
        channel.queueDeclare(QUEUE_NAME,false,false,false,null);
        //保证发送给消费者的消息只分发一次
        channel.basicQos(1);
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
            //给一个消费者开启一点延时,模拟性能问题
            try {
                Thread.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //开启手动消息应答
            channel.basicAck(delivery.getEnvelope().getDeliveryTag(),false);
        };
        /*
            消费队列里面的消息
            参数介绍: 这里只说第2个参数，其他参数前面已经说过
            第2个参数 autoAck : false
         */
        channel.basicConsume(QUEUE_NAME,false,deliverCallback,consumerTag -> {});
    }
}
```

* 结果展示

```
运行上面的代码打印出来的消息为:

消息提供者:发送10条消息
    [x] Sent 'Hello RabbitMQ i=0'
    [x] Sent 'Hello RabbitMQ i=1'
    [x] Sent 'Hello RabbitMQ i=2'
    [x] Sent 'Hello RabbitMQ i=3'
    [x] Sent 'Hello RabbitMQ i=4'
    [x] Sent 'Hello RabbitMQ i=5'
    [x] Sent 'Hello RabbitMQ i=6'
    [x] Sent 'Hello RabbitMQ i=7'
    [x] Sent 'Hello RabbitMQ i=8'
    [x] Sent 'Hello RabbitMQ i=9'

消息消费者1:(消费者1具体处理几条不一定)
    [x] Received 'Hello RabbitMQ i=1'
    [x] Received 'Hello RabbitMQ i=2'
    [x] Received 'Hello RabbitMQ i=4'
    [x] Received 'Hello RabbitMQ i=5'
    [x] Received 'Hello RabbitMQ i=6'
    [x] Received 'Hello RabbitMQ i=8'
    [x] Received 'Hello RabbitMQ i=9'

消息消费者2:(消费者2具体处理几条不一定)
    [x] Received 'Hello RabbitMQ i=0'
    [x] Received 'Hello RabbitMQ i=3'
    [x] Received 'Hello RabbitMQ i=7'

通过以上代码运行可以看出不在是基本的轮询，而是采用了公平分发的原则。
```

---

### 第5节 消息队列的持久化

#### 5.1 消息持久化介绍

```
上面介绍的队列都是在RabbitMQ退出或者崩溃时就会消失的队列,如果当时队列里面还有消息未被消费,那么异常退出会造成任务丢失,怎么保证就算RabbitMQ意外退出,也不会造成队列中的任务丢失呢,这回就用到了消息队列的持久化.
```
---
#### 5.2 消息持久化设置

```
设置持久化队列比较简单只需要在提供者和消费者都设置
//第二个参数设置为true
channel.queueDeclare(QUEUE_NAME,true,false,false,null);

这样当我们消息发布者发布一条消息之后然后重启docker RabbitMQ服务器,查看web管理页面会发现,消息还在

重启docker容器的命令: docker container restart bf44c7a5089a
```
---
#### 5.3 消息持久化实现

* 消息发送代码

```java
/**
 * 向队列中发送一个消息
 */
public class ProducerMessage {
    //声明一个队列名称
    private static final String QUEUE_NAME="hello";
    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            声明一个持久化队列
            参数说明:第2个参数: durable: 设置为true,这时我们声明的队列就是一个持久化队列
         */
        channel.queueDeclare(QUEUE_NAME,true,false,false,null);
        //构建一个消息
        String message = "Hello RabbitMQ";
        channel.basicPublish("",QUEUE_NAME,null,message.getBytes());
        System.out.println(" [x] Sent '" + message + "'");
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 消息消费代码

```java
/**
 * 从队列中获取消息
 */
public class ConsumerMessage {
    //声明要获取消息的队列名称
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        //声明一个队列,第2个参数,设置为true声明一个持久化队列
        channel.queueDeclare(QUEUE_NAME,true,false,false,null);

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        channel.basicConsume(QUEUE_NAME,true,deliverCallback,consumerTag -> {});
    }
}
```

---
## 第六章 发布/订阅(RabbitMQ-Publish&Subscribe)


### 第1节 Publish&Subscribe原理图

|            Publish&Subscribe             |
| :--------------------------------------: |
| ![python-four](https://note.youdao.com/yws/api/personal/file/F597EF30C192405787080AB890611CA0?method=download&shareKey=be3b46c9d51c7386649f03285c0422fc) |

---
### 第2节 Publish&Subscribe介绍

```
在前面的例子中,我们创建了一个工作队列,都是一个任务只交给一个消费者.这次我们做一些完全不同的事儿.将消息发送给多个消费者,这种模式叫做 "发布/订阅"
```
---
### 第3节 Exchanges(交换器)
```
要想实现上面的发布/订阅这种模式,需要使用到交换器
```
---

#### 3.1 交换器是干什么的

> 前面的例子,我们都是基于一个队列发送和接收消息.现在介绍一下完整的消息传递模式

```
RabbitMQ消息模式的核心理念:
1. 生产者没有直接发送任何消费到队列
2. 实际上,生产者都不知道这个消费是发送给哪个队列的
3. 相反,生产者只需发送消息给交换器,交换器一方面它接受生产者的消息,另一方面向队列推送消息
4. 交换器必须清楚的知道如何处理接收到的消息,指定一个队列或者指定多个队列或者是直接丢弃消息等处理方式,这些规则通过交换器的类型进行定义
```

---
#### 3.2 交换器的常见类型

```
常见的交换器类型有: direct、topic、headers 和 fanout

下面会介绍常见类型的交换器
```
---
#### 3.3 交换器的创建
```
channel.exchangeDeclare("logs", "fanout"); //使用channel对象创建一个名为logs，类型为fanout交换器
```
---
#### 3.4 匿名交换器和临时队列

* 匿名交换器
```
上面两个例子我们没有使用交换器,但是也可以将消息发送到队列，那是因为我们使用了默认的交换器,交换器名字为空字符串

代码: channel.basicPublish("", "hello", null, message.getBytes()); 第1个参数空字符串就是交换器名称,现在创建了一个类型为fanout名字为logs的交换器,可以指定交换器的名字了

代码: channel.basicPublish("logs","",null,message.getBytes());
```
* 临时队列

```
我们上面的例子在使用队列的时候都会指定一个名字,队列有名字对我们来说是非常重要的因为我们需要为消费者指定同一个队列去消费消息,但是接下来对于我们要完成的日志系统的例子来说指定具体队列不是我们所必须的，我们只关心消息发布出来之后消费者全部都接收到,不需要指定一个具体队列,使用临时队列即可.首先每当我们连接到RabbitMQ的时候需要为我们需要创建一个随机名字的空队列,其次,一旦消费者断开连接,队列将自动删除.


创建一个非持久化、独立的、自动删除且名字是随机生成的临时队列的方式: String queueName = channel.queueDeclare().getQueue();

queueName是一个随机队列名 比如生成一个amq.gen-JzTY20BRgKO-HjmUJj0wLg名字
```
---
#### 3.5 队列绑定

|                     队列绑定                     |
| :----------------------------------------------: |
| ![python-bindings](https://note.youdao.com/yws/api/personal/file/C01B8A01CE52497680E92F5F1D724EA6?method=download&shareKey=526761beb2b1d94d9c53796b8167247a) |

---
### 第4节 Publish&Subscribe实现

> 练习 消息的发布/订阅

```
构建一个简单日志系统.它包含2段程序:第一个是消息提供者,将发布日志消息,第二个是消息消费者接受并打印消息,当然需要启动多个消费者,验证多个消费者同时订阅消息,这个模式类似于我们的广播(一个电台播放音乐,每个人订阅这个电台频道,都可以收到这个电台放的音乐,对应我们的需求,就是发布者发布消息,可以被所有的消费者接收到)

注意: 在启动下面代码示例时,首先要先启动消费者的两个客户端代码,其次启动提供者客户端的代码
```

* 发布消息(消息提供者)代码

```java
/**
 * 消息发布代码
 */
public class PublishMessageLog {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为fanout交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
        //向交换器中发送几条消息,我这里循环发送三条消息
        for (int i = 1; i <=3; i++) {
            //构建发送的消息
            String message = "Hello RabbitMQ "+i;
            //发送
            channel.basicPublish(EXCHANGE_NAME,"",null,message.getBytes());
        }
        System.out.println("========消息发布完成========");
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

```
生产者声明了一个广播模式的交换器,订阅这个交换器的消费者都可以收到每一条消息.可以看到在生产者中,没有声明队列.这也验证了之前说的.生产者其实只关心exchange,至于exchange会把消息转发给哪些队列,并不是生产者关心的
```


* 订阅消息(消息消费者)代码

```java
//在这里使用两个消费者测试,一个消费者将订阅到的消息直接打印到控制台,另一个消费者将订阅到的消息保存为本地日志文件

//消费者1
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog1 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端1");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为fanout交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();
        //将交换器和随机队列绑定到一起
        channel.queueBind(queueName,EXCHANGE_NAME,"");

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            System.out.println("订阅到的消息为: "+message);
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}



//消费者2
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog2 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端2");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为fanout交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();
        //将交换器和随机队列绑定到一起
        channel.queueBind(queueName,EXCHANGE_NAME,"");

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            //将获取到的日志信息保存为本地文件 文件名为abc.log
            FileOutputStream fos = new FileOutputStream(new File("abc.log"), true);
            fos.write(message.getBytes());
            fos.flush();
            //关闭
            fos.close();
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}
```

```
2个消费者,一个打印日志,一个写入文件,消费者实例启动后,会创建一个随机队列,这个在管理页面可以看到(如下图).而消费者实例关闭后,随机队列也会自动删除(所以需要先启动消费者创建随机队列,后启动生产者)
```

* 管理控制台查看

|                    管理页面                    |
| :--------------------------------------------: |
| ![RabbitMQ-info3](https://note.youdao.com/yws/api/personal/file/344E988DBA584222AE133C4CE389578D?method=download&shareKey=07e8b064c7affa6f996d5dd8f4be815e) |

---
## 第七章 路由(RabbitMQ-Route)

### 第1节 原理图

|                 Route                  |
| :------------------------------------: |
| ![python-six](https://note.youdao.com/yws/api/personal/file/27C56AD394AA4B339F170DC425AED42C?method=download&shareKey=a33eda00e49a812cf7238ac294dc4287) |

---
### 第2节 Route(路由)介绍

* Route的功能

```
1. 在上个消息"发布/订阅"案例中我们建立了一个简单的日志系统,可以广播消息给多个消费者
2. 在这个案例中我们将添加新的特性.我们可以只订阅部分消息.比如:我们的日志分多个级别(info,debug,error)
3. 如果是上面的单纯的"发布/订阅"模式,那么我们会把所有级别的日志都打印到控制台或者保存到本地日志文件中
4. 但是对于有些级别的日志我们不需要保存到本地硬盘,占用空间,这时候就需要对发布者发布的消息进行过滤,进行有针对性的消费,这就需要我们的Route帮助我们完成.
```
* 交换器的选择

```
前面讲到我们的日志系统广播消息给所有的消费者.我们想对其扩展,根据消息的严重性来过滤消息.我们使用的fanout交换器,不能给我们太多的灵活性.它仅仅只是盲目的广播而已.我们使用direct交换器进行代替,其背后的算法很简单,消息会被推送至绑定键(routingKey)和消息发布附带的选择键(routingKey)完全匹配的队列
```

* Route的配置
```
1. 消息发布
    1.1 交换器类型 channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT)
    1.2 发布消息的参数 channel.basicPublish(EXCHANGE_NAME,"routingKey",null,message.getBytes())

2. 消息订阅
    2.1 交换器类型 channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT)
    2.2 队列绑定 channel.queueBind(queueName, EXCHANGE_NAME, severity) //可以进行多重绑定
```
---
### 第3节 Route(路由)实现

* 发布消息(消息提供者)代码

```java
/**
 * 消息发布代码
 */
public class PublishMessageLog {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号,如果不设置为默认值
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为direct交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);

        /*
            模拟发送多个级别的日志库[info,debug,error]
         */
        String[] severities={"error","info","warning"};
        for (int i = 0; i < severities.length; i++) {
            //模拟待发送的消息
            String message = "Hello RabbitMQ "+i+"\n";
            //发送消息,每次发送之前指定 routingKey: 使用severities里面的值标记每条发送消息的日志级别(就是给每条消息设置路由key)
            channel.basicPublish(EXCHANGE_NAME,severities[i],null,message.getBytes());
        }
        System.out.println("========消息发布完成========");
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 订阅消息(消息消费者)代码

```java
//客户端1
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog1 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端1");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为direct交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();

        /*
            消费者1采用多重绑定的方式,订阅所有的日志级别的信息
         */
        String[] severities={"error","info","warning"};
        for (String severity : severities) {
            channel.queueBind(queueName,EXCHANGE_NAME,severity);
        }


        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            System.out.println("订阅到的消息为: "+message);
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}


//客户端2
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog2 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端2");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为direct交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();
        //消费者2只绑定日志级别为error的消息
        String routingKey = "error";
        channel.queueBind(queueName,EXCHANGE_NAME,routingKey);

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            //将获取到的日志信息保存为本地文件 文件名为abc.log
            FileOutputStream fos = new FileOutputStream(new File("abc.log"), true);
            fos.write(message.getBytes());
            fos.flush();
            //关闭
            fos.close();
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}
```

```
上面说了很多,其实就为了做一件事,我们可以使用Direct exchange+routingKey来过滤自己感兴趣的消息,一个队列可以绑定多个routingKey,这就是有选择

注意:
    1. 启动的时候还是需要先启动消费者端,后启动提供者端
    2. 如果当前路由选择的案例延续使用消息"发布/订阅"案例需要注意不要使用原有的队列,我们新的参数在原有队列不生效
    3. 解决方法,重新声明队列,或者重启RabbitMQ docker容器
        - 关闭容器命令 docker contanier stop "CONTAINER ID"
        - 启动容器命令 docker contanier start "CONTAINER ID"
        - 重启容易命令 docker contanier restart "CONTAINER ID"
```
---
## 第八章 主题(RabbitMQ-Topic)

### 第1节 Topic(主题)原理图

|                    Topic                     |
| :------------------------------------------: |
| ![python-serven](https://note.youdao.com/yws/api/personal/file/33D7EA24E9394502AF326BEFAEEFC14F?method=download&shareKey=0ca2a435211c05d1c411e24d5cddb2ed) |

---
### 第2节 Topic(主题)介绍

* Topic(主题)交换器的功能

```
在上一节中我们使用了direct类型的交换器改进了日志系统,但是还是具有一定的局限性,不能根据多重条件进行路由选择,在我们的日志系统中，我们可能不仅仅根据日志严重性(info/warning/error)订阅日志，也想根据日志来源(auth/cron/kern)订阅日志，这将给我们带来更大的灵活性.比如我们可以订阅auth来源的error级别日志,还可以订阅cron来源的所有级别日志,这就需要我们强大的主题类型的交换器
```

* Topic(主题)交换器使用

```
我们主题交换器的使用和上面的Route(路由交换器)使用是类似的,都需要指定routingKey,只是发送给主题交换器的消息不能是任意设置的routingKey，必须是用小数点隔开的一系列的标识符.这些标识符可以是随意,但是通常跟消息的某些特性相关联.合法的routingKey 比如"socket.usd.nyse","nyse.vmw","quick.orange.rabbit",你愿意用多少单词都可以,只要不超过上限的255个字节,关于routingKey有两种特殊的情况：*(星号)可以代替任意一个标识符; #(井号)可以代替零个或多个标识符(单词)
```
```
消息发布:

    1. 交换器设置为topic channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
    2. 构建routingKey并发布 channel.basicPublish(EXCHANGE_NAME,routingKey,null,message.getBytes());

消息订阅:

    1. 交换器设置为topic channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
    2. 绑定设置好订阅规则的routingKey channel.queueBind(queueName,EXCHANGE_NAME,routingKey);
```
---
### 第3节 Topic(主题)实现

* 发布消息(消息提供者)代码

```java
/**
 * 消息发布代码
 */
public class PublishMessageLog {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";

    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号,如果不设置为默认值
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为topic交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
        /*
            模拟多个设备["auth","cron","kern"]
            模拟级别的日志库[info,debug,error]
         */
        String[] facilities ={"auth","cron","kern"};
        String[] severities={"error","info","warning"};

        //模拟发送所有设备(facilities)的所有级别(severities)的日志
        for (int i = 0; i < facilities.length; i++) {
            for (int j = 0; j <severities.length; j++) {
                /*
                    拼装routingKey
                    格式为: auth.info  auth.waring auth.error
                 */
                String routingKey = facilities[i]+"."+severities[j];
                // 设置发送的消息
                String message = "["+facilities[i]+"."+severities[j]+"] Hello RabbitMQ i=" + i;
                //发送
                channel.basicPublish(EXCHANGE_NAME,routingKey,null,message.getBytes());
                System.out.println("消息提供者发送的日志消息: "+message);
            }
        }
        System.out.println("========消息发布完成========");
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

* 订阅消息(消息消费者)代码

```java
// 消费者1
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog1 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端1");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为topic交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();
        /*
            消费者1采用多重绑定的方式,订阅所有设备的日志级别的信息
            所以我们需要构建routingKey来订阅发布者发布的信息
            使用通配符过滤哪些消息被丢弃，哪些消息被消费打印到控制台
            auth.*    : 订阅auth系统后面是一个单词的日志 比如 auth.info auth.warning auth.error
            *.info    : 订阅info级别的并且前面是一个单词的日志 比如 auth.info等 如果前面有两个单词的如auth.abc.info 不会被订阅到
            #.warning : 订阅所有系统的warning的级别日志，因为#匹配多个单词
         */
        String[] routingKeys ={"auth.*","*.info","#.warning"};
        for (String routingKey : routingKeys) {
            channel.queueBind(queueName,EXCHANGE_NAME,routingKey);
        }

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            System.out.println("订阅到的消息为: "+message);
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}


// 消费者2
/**
 * 消息订阅代码
 */
public class SubscribeMessageLog2 {
    //声明需要创建的交换器名称
    private final static String EXCHANGE_NAME = "logs";
    public static void main(String[] args) throws IOException, TimeoutException {
        System.out.println("消费者客户端2");
        //创建连接RabbitMQ服务器的连接
        ConnectionFactory factory = new ConnectionFactory();
        //设置连接RabbitMQ的服务器地址
        factory.setHost("192.168.1.129");
        //设置连接RabbitMQ的服务器AMQP端口号
        factory.setPort(5672);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建频道
        Channel channel = connection.createChannel();
        /*
            创建一个名为logs，类型为topic交换器
            RabbitMQ客户端提供了交换器类型的枚举BuiltinExchangeType
         */
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
        //声明一个随机队列
        String queueName = channel.queueDeclare().getQueue();
        //消费者2只订阅 auth系统级别为error的日志消息
        String routingKey = "auth.error";
        channel.queueBind(queueName,EXCHANGE_NAME,routingKey);

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            //获取订阅到的消息
            String message = new String(delivery.getBody());
            //将获取到的日志信息保存为本地文件 文件名为abc.log
            FileOutputStream fos = new FileOutputStream(new File("abc.log"), true);
            fos.write(message.getBytes());
            fos.flush();
            //关闭
            fos.close();
        };
        //获取订阅到的消息
        channel.basicConsume(queueName,true,deliverCallback,consumerTag -> { });
    }
}
```

* 结果

```
通过上面的例子的结果

发布者发布的消息:
    消息提供者发送的日志消息: [auth.error] Hello RabbitMQ i=0
    消息提供者发送的日志消息: [auth.info] Hello RabbitMQ i=0
    消息提供者发送的日志消息: [auth.warning] Hello RabbitMQ i=0
    消息提供者发送的日志消息: [cron.error] Hello RabbitMQ i=1
    消息提供者发送的日志消息: [cron.info] Hello RabbitMQ i=1
    消息提供者发送的日志消息: [cron.warning] Hello RabbitMQ i=1
    消息提供者发送的日志消息: [kern.error] Hello RabbitMQ i=2
    消息提供者发送的日志消息: [kern.info] Hello RabbitMQ i=2
    消息提供者发送的日志消息: [kern.warning] Hello RabbitMQ i=2

消费者1消费的消息:
	将符合routingKey的消息消费了 {"auth.*","*.info","#.warning"}
	消息如下:
        订阅到的消息为: [auth.error] Hello RabbitMQ i=0
        订阅到的消息为: [auth.info] Hello RabbitMQ i=0
        订阅到的消息为: [auth.warning] Hello RabbitMQ i=0
        订阅到的消息为: [cron.info] Hello RabbitMQ i=1
        订阅到的消息为: [cron.warning] Hello RabbitMQ i=1
        订阅到的消息为: [kern.info] Hello RabbitMQ i=2
        订阅到的消息为: [kern.warning] Hello RabbitMQ i=2

消费者2消费的消息:
	将符合routingKey的消息消费了 "auth.error"
	消息如下:
		[auth.error] Hello RabbitMQ i=0[auth.error] Hello RabbitMQ i=0
```

---
## 第九章 SpringBoot整合RabbitMQ
```
创建SpringBoot项目,并且选择RabbitMQ的依赖,由于不同的时间段可能SpringBoot和RabbitMQ的版本略微不同

jdk版本        : 1.8
springboot版本 : 2.1.13.RELEASE
```

### 第1节 SpringBoot和RabbitMQ整合并配置

```
在application.properties配置文件中配置基本的配置信息

#设置MQ服务器地址
spring.rabbitmq.host=192.168.1.129
#设置MQ的端口号
spring.rabbitmq.port=5672
#开启ACK 自动/手动
spring.jms.listener.acknowledge-mode=auto
```
---
### 第2节 代码实现

#### 2.1 不设置交换器

* RabbitMQ configuration配置

```
@Configuration
public class RabbitMQConfig {
    //普通队列名称
    private static final String QUEUE_SIMPLE="HELLO_QUEUE";
    //持久化队列名称
    private static final String QUEUE_PERSIST ="WORK_QUEUE";

    //创建一个普通队列(非持久化) 导包 org.springframework.amqp.core.Queue 别导错包
    @Bean("simpleQueue")
    public Queue createSimpleQueue(){
        return new Queue(QUEUE_SIMPLE,false);
    }
    //创建一个持久化队列
    @Bean("workQueue")
    public Queue createWorkQueue(){
        return new Queue(QUEUE_PERSIST,true);
    }

}
```

* 消息发布和消费
* * 定义一个User的POJO类用于作为消息发送

```
//类一定序列化
public class User implements Serializable {
    private Integer userId;
    private String userName;
    private Date createTime;
    private double salary;
    //省略get/set方法
}
```

* * 消息发布

```
@Component
public class ProducerMessage {
    @Resource
    private Queue simpleQueue; //注入配置中的普通队列
    @Resource
    private Queue workQueue;   //注入配置中的持久化队列
    @Resource
    private AmqpTemplate amqpTemplate;// RabbitMQ使用AmqpTemplate进行消息发布
    /**
     * 发送一个User对象到普通队列
     */
    public void sendUser(User user){
        //指定队列名称
        amqpTemplate.convertAndSend(simpleQueue.getName(),user);
        System.out.println("消息发送成功...........");
    }
    /**
     * 发送一个User集合到持久化队列
     */
    public void sendUserList(List<User> userList){
        //指定队列名称
        amqpTemplate.convertAndSend(workQueue.getName(),userList);
        System.out.println("消息发送成功...........");
    }
}
```

* * 消息订阅

```
@Component
public class ConsumerMessage {
    /**
     * 监听消费普通队列的消息
     */
    @RabbitListener(queues = {"HELLO_QUEUE"})
    public void receiveUser(User user){
        System.out.println("接收到的user:"+user);
    }
    /**
     * 监听消费持久化队列里面的消息
     */
    @RabbitListener(queues = {"WORK_QUEUE"})
    public void receiveUserList(List<User> users){
        System.out.println("接收到的users:"+users);
    }
}
```

* * 测试

```
//测试的时候只需要测试消息发布，不需要写消息接受的测试,因为消息消费是监听器(RabbitListener),是时时的,在我们发送完成之后，直接就会消费，不需要在写测试类，去消费。

@RunWith(SpringRunner.class)
@SpringBootTest
public class ProducerMessageTest{
    @Resource
    private ProducerMessage producerMessage;

    @Test
    public void sendUser() {
        User user = new User();
        user.setUserId(1001);
        user.setSalary(30000.12);
        user.setUserName("李雷");
        user.setCreateTime(new Date());
        producerMessage.sendUser(user);
    }
    @Test
    public void sendUserList() {
        List<User> userList = new ArrayList<>();
        User user1 = new User();
        user1.setUserId(1001);
        user1.setSalary(30000.12);
        user1.setUserName("李雷");
        user1.setCreateTime(new Date());
        User user2 = new User();
        user2.setUserId(1002);
        user2.setSalary(320000.12);
        user2.setUserName("李雷2");
        user2.setCreateTime(new Date());
        userList.add(user1);
        userList.add(user2);
        producerMessage.sendUserList(userList);
    }
}

//运行测试之后的部分日志,可以看到我们运行的提供者，但是消费者的消费消息也打印出来了。
消息发送成功...........
接收到的 users:[User{userId=1001, userName='李雷', createTime=Fri Apr 03 20:23:00 CST 2020, salary=30000.12}, User{userId=1002, userName='李雷2', createTime=Fri Apr 03 20:23:00 CST 2020, salary=320000.12}]
```

---
#### 2.2 设置交换器

* RabbitMQ configuration配置

```
@Configuration
public class RabbitMQConfig {

    //队列名称
    private static final String QUEUE_NAME ="queueWork";
    //交换器名称
    private static final String EXCHANGE_NAME="LOGS";
    //声明需要订阅的routingKey类型
    private static final String ROUTING_KEY="*.error";

    //创建一个队列
    @Bean("queueWork")
    public Queue createworkQueue(){
        return new Queue(QUEUE_NAME,true);
    }
    //创建一个TOPIC类型交换器
    @Bean("topicExchange")
    public TopicExchange createTopicExchange(){
        return new TopicExchange(EXCHANGE_NAME);
    }
    //将队列、交换器和ROUTING_KEY绑定,如果想绑定多个ROUTING_KEY可以定义多个bindQueueExchageKey方法
    @Bean
    public Binding bindQueueExchageKey(){
        return BindingBuilder.bind(createworkQueue()).to(createTopicExchange()).with(ROUTING_KEY);
    }

}
```

* 消息发布和消费
* * 消息发布

```
@Component
public class ProducerMessage {
    @Resource
    private Exchange topicExchange;// topic类型的交换器
    @Resource
    private AmqpTemplate amqpTemplate;// 消息处理的对象

    /**
     * 使用topic类型交换器
     * 发送多个设备多种类型的消息
     */
    public void sendLevelMessage(){
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app1.info","我app1.info下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app1.waring","我app1.waring下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app1.error","我app1.error下的消息");

        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app2.info","我app2.info下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app2.waring","我app2.waring下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app2.error","我app2.error下的消息");

        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app3.info","我app3.info下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app3.waring","我app3.waring下的消息");
        amqpTemplate.convertAndSend(topicExchange.getName(),
                                    "app3.error","我app3.error下的消息");
    }

}
```

* * 消息消费

```
@Component
public class ConsumerMessage {
    /**
     * 消费消息
     */
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(value = "queueWork"),
            exchange = @Exchange(value = "topicExchange")))
    public void receiveErrorLogs(Message message){
        System.out.println("获取到的error消息为:"+ new String(message.getBody()));
    }
}
```

* * 测试
```
@RunWith(SpringRunner.class)
@SpringBootTest
public class ProducerMessageTest{
    @Resource
    private ProducerMessage producerMessage;
    @Test
    public void sendLevelMessage(){
        producerMessage.sendLevelMessage();
    }
}

//日志中的打印结果:
/**
 * 获取到的error消息为:我app1.error下的消息
 * 获取到的error消息为:我app2.error下的消息
 * 获取到的error消息为:我app3.error下的消息
 */
```
---
## 第十章 RabbitMQ事务&Confirm

### 第1节 RabbitMQ事务机制
```
1. 通过上面的学习大家都知道我们的RabbitMQ的队列是可以持久化保存数据的，就算我们的MQ服务器挂掉或者重启也不会造成数据丢失,但是如果我们的消息还没有到队列就丢失了怎么办呢？

2. 接下来我们就说一下RabbitMQ怎么解决这个问题,如果不做任何配置的情况下我们使用消息提供者发布消息,消息发布完成之后其实提供者是不知道消息发送是否成功的,是否正确到达broker的，RabbitMQ提供了两种方式来解决这个问题:

    第1种: 通过AMQP事务机制实现
    第2种: 通过将channel设置成confirm模式来实现
```
---
### 第2节 AMQP事物机制

#### 2.1 AMQP事物机制介绍

```
该模式与数据库的事务非常相似。RabbitMQ中与事务机制有关的方法有txSelect(),txCommit()以及txRollback()。txSelect用于将当前channel设置成transaction模式，txCommit用于提交事务，txRollback用于回滚事务。在通过txSelect开启事务之后，我们便可以发布消息给broker了，如果txCommit提交成功了，则消息一定到达了broker了，如果在txCommit执行之前broker异常崩溃或者由于其他原因抛出异常，这个时候我们便可以捕获异常通过txRollback回滚事务了
```

---
#### 2.2 AMQP事物机制实现

* 代码如下

```
public class ProducerAMQP {

    public static void main(String[] args) throws IOException, TimeoutException {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.1.129");
        factory.setPort(5672);
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
        //声明一个持久化队列
        channel.queueDeclare("AMQP_QUEUE",true,false,false,null);

        try {
            //创建消息
            String message="AMQP事物消息";
            //开启事务
            channel.txSelect();
            //发布消息
            channel.basicPublish("","AMQP_QUEUE",true, MessageProperties.PERSISTENT_TEXT_PLAIN,message.getBytes());
            //用于测试的异常
            int i=1/0;
            //提交事务
            channel.txCommit();
            System.out.println("消息发送成功...........");
        }catch (RuntimeException e){
            System.out.println("消息发送失败...........");
            //回滚事务
            channel.txRollback();
        }
    }
}


该模式用法简单,但是有个致命的缺点,那就是事务提交非常慢,会严重降低系统吞吐量,所以一般不推荐使用该模式,而改用confirm模式
```

---
### 第3节 Confirm机制

```
Confirm发送方确认模式使用和事务类似,也是通过设置Channel进行发送方确认的,并且Confirm支持异步.


Confirm的三种实现方式:
方式一: channel.waitForConfirms()普通发送方确认模式
方式二: channel.waitForConfirmsOrDie()批量确认模式
方式三: channel.addConfirmListener()异步监听发送方确认模式


我们这里只演示最后一种常用的异步监听发送方式:
    - 消息发布者发布多条消息,并使用异步监听
    - 消息订阅者消息确认模式(ack)采用手动模式
    - prefetch设置为1，限制通道上未完成的发送消息数量
```

* 消息提供者代码如下

```
public class ProducerConfirm {
    public static void main(String[] args) throws IOException, TimeoutException {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.1.129");
        factory.setPort(5672);
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
        //声明一个持久化队列
        channel.queueDeclare("CONFIRM_QUEUE",true,false,false,null);
        //开启发送方确认模式
        channel.confirmSelect();
        //发送消息
        //创建消息
        String message = "Hello RabbitMQ Confirm ";
        channel.basicPublish("","CONFIRM_QUEUE",MessageProperties.PERSISTENT_TEXT_PLAIN,message.getBytes());
        System.out.println("发送消息为: "+message);


        //监听发送状态
        channel.addConfirmListener(new ConfirmListener() {
            /*
                消息确认成功
                deliveryTag: 如果发送多条，这是最后一条消息的deliveryTag
             */
            @Override
            public void handleAck(long deliveryTag, boolean multiple) throws IOException {
                System.out.println("已确认消息: "+deliveryTag);
            }
            //消息确认异常
            @Override
            public void handleNack(long deliveryTag, boolean multiple) throws IOException {
                System.out.println("未确认消息: "+deliveryTag);
            }
        });

    }
}
```
* 消息消费者代码如下

```
public class ConsumerConfirm {
    public static void main(String[] args) throws IOException, TimeoutException {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("192.168.1.129");
        factory.setPort(5672);
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
        //声明一个持久化队列
        channel.queueDeclare("CONFIRM_QUEUE",true,false,false,null);
        //设置prefetch
        int prefetch=1;
        channel.basicQos(prefetch);
        //消费消息
        boolean autoAck = false; //关闭消息的自动确认
        channel.basicConsume("CONFIRM_QUEUE", autoAck, new DeliverCallback() {
            @Override
            public void handle(String consumerTag, Delivery message) throws IOException {
                //获取消息内容
                byte[] body = message.getBody();
                System.out.println("消息已被消费: "+new String(body));
                //获取消息序号
                long deliveryTag = message.getEnvelope().getDeliveryTag();
                System.out.println("消息已被消费 deliveryTag: "+deliveryTag);
                /*
                    手动确认此序号的消息
                    multiple:如果设置为false那么只确认当前程序的消息,如果设置为true，确认当前序号之前的所有消息
                 */
                channel.basicAck(deliveryTag,true);
            }
        }, (consumerTag) -> {});

    }
}
```

> 以上为RabbitMQ的常见操作,信息来源于官网,如果有什么翻译或者讲解错误,欢迎发送邮件指正.
---