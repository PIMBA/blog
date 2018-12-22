---
title: Rabbit MQ 开箱试玩
date: 2018-04-12 22:25:35
tags:
---
# 什么是MQ

MQ = Message Queue ,中文名为消息队列。是一种进程间或不同的线程间进行通讯的一种方式。
在实际的生产当中，我们常常需要用到消息队列对消息生产者和消息消费者进行解耦，提升系统的稳定新或性能。
从字面上理解消息队列非常容易，它只是一个队列，存放消息发送者发送的消息，等待消费者进行消费，但现代的消息队列产品提供了更多的功能，包括集群化，高可用性主备模式，消息广播等等功能。

# 为什么使用MQ

要解释为什么使用MQ，我们可以以一个故事来阐述：

> 小明是一个大懒虫，不喜欢读书，但小明的姐姐小红是很喜欢读书。为了督促小明学习，小红决定每个月给小明一本书让他看完。
第一个月，小红给了小明第一本书，**并看着他把这本书读完了**。
第二个月，小红给了小明第二本书，**并看着他把这本书读完了**。
第三个月，小红觉得老是看着他读不是一个办法，她决定把第三本书交给小明，并约定下个月给他下一本。
第四个月，小明因为偷懒没有看完第三本书，小红开始**每天问他什么时候能够看完**。
这时，小红觉得老是这样问不是个办法，她决定每个月**将书放在小明房间门口的书架上，并让小明把读书笔记也放在书架上**。
这样，小红可以每天读她自己的书，时不时的去看看读书笔记还在不在，并将下一本书放在书架上即可。

在上面这个故事中，*小明房间门口的书架*就是消息队列，从这个故事中，我们可以看出，使用消息队列可以将消息的生产和消费完全解耦，并将生产者和消费者之间复杂的沟通关系通过一个容器变为生产者与容器，消费者与容器之间的简单关系，将消息的消费变为一个异步的行为，提升系统的整体性能。并且，消息队列对于消息的缓存机制也提升了系统的稳定性。

市面上有很多消息队列(中间件)产品，比如Apache家的ActiveMQ,IBM家的IBM MQ,还有Rabbit MQ,我们今天就来简单的试玩一下 Rabbit MQ。

# Rabbit MQ

[Rabbit MQ](https://www.rabbitmq.com/) 是一个由RabbitMQ Technologies Ltd使用[erlang](https://www.erlang.org/)开发的AMQP协议的开源实现，AMQP是一个公开的消息协议，在2006年6月，Cisco 、Redhat、iMatix 等联合制定了 AMQP 的公开标准。
值得一提的是，2010年4月，VMWare收购了RabbitMQ，而收购它的部门，就是SpringSource。

具体Rabbit MQ 可以做到什么，可以参见[官网](https://www.rabbitmq.com/)

# 安装(linux-centos7)

安装 rabbit MQ 需要 erlang 环境，所以我们首先安装erlang
```shell
wget http://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
rpm -Uvh erlang-solutions-1.0-1.noarch.rpm
rpm --import http://packages.erlang-solutions.com/rpm/erlang_solutions.asc
```

添加 RPMforge支持(64位)
```shell
wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm
rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt
rpm -i rpmforge-release-0.5.2-2.el6.rf.*.rpm
```

安装erlang
```shell
yum install erlang
```

然后可以检查erlang版本
```shell
erl -version
```

现在可以开始安装 rabbit MQ了

首先从官网下载你想要的版本，这里使用3.51
```shell
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.5.1/rabbitmq-server-3.5.1-1.noarch.rpm
```

引入包然后安装:
```shell
rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
yum install rabbitmq-server-3.5.1-1.noarch.rpm
```

# 启停

为了服务器挂了之后还能自动重启，我们将服务配置为随系统启动
```shell
chkconfig rabbitmq-server on
```

启动
```shell
service rabbitmq-server start
```
> 如果出现`Starting rabbitmq-server (via systemctl):  Job for rabbitmq-server.service failed. See 'systemctl status rabbitmq-server.service' and 'journalctl -xn' for details. [FAILED]` 需要把SELinux关掉:修改 `/etc/selinux/config` : `SELINUX=disabled`并重启系统

停止
```shell
service rabbitmq-server stop
```

至此，rabbitmq在centos上的安装就已经完成了，我们可以通过外部程序连接mq了。

# web管理界面

安装
```shell
rabbitmq-plugins enable rabbitmq_management
```

然后访问15672端口即可

# 简单使用

首先新建一个maven工程，然后添加一下依赖:
```xml
<!-- rabbitmq相关依赖 -->
<dependency>
      <groupId>com.rabbitmq</groupId>
      <artifactId>amqp-client</artifactId>
      <version>3.0.4</version>
</dependency>
```

如果你需要序列化的库，可以用这个:
```xml
<!-- 序列化相关依赖 -->
<dependency>
    <groupId>commons-lang</groupId>
    <artifactId>commons-lang</artifactId>
    <version>2.6</version>
</dependency>
```

编写以下代码测试是否连接
生产者
```java
public final class Main {
    private final static String HOST = "127.0.0.1";
    private final static String QUEUE = "queue";
    private final static String NAME = "admin";
    private final static String PASS = "admin";

    public static void main(String[] agrs) throws IOException {
        /**
         * 创建连接连接到RabbitMQ
         */
        ConnectionFactory factory = new ConnectionFactory();
        //设置RabbitMQ所在主机ip或者主机名
        factory.setHost(HOST);
        factory.setUserName(QUEUE);
        factory.setPassword(PASS);
        //创建一个连接
        Connection connection = factory.newConnection();
        //创建一个频道
        Channel channel = connection.createChannel();
        //指定一个队列
        channel.queueDeclare(QUEUE, false, false, false, null);
        //发送的消息
        String message = "hello world!";
        //往队列中发出一条消息
        channel.basicPublish("", QUEUE, null, message.getBytes());
        //关闭频道和连接
        channel.close();
        connection.close();
    }
}
```

消费者
```java
public class Recv  {
    //队列名称
    private final static String QUEUE_NAME = "queue";
    public static void main(String[] argv) throws java.io.IOException,
    java.lang.InterruptedException
    {
        //打开连接和创建频道，与发送端一样
        ConnectionFactory factory = new ConnectionFactory();
        //设置MabbitMQ所在主机ip或者主机名
        factory.setHost("127.0.0.1");
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
        //声明队列，主要为了防止消息接收者先运行此程序，队列还不存在时创建队列。
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        //创建队列消费者
        QueueingConsumer consumer = new QueueingConsumer(channel);
        //指定消费队列
        channel.basicConsume(QUEUE_NAME, true, consumer);
        while (true){
            //nextDelivery是一个阻塞方法（内部实现其实是阻塞队列的take方法）
            QueueingConsumer.Delivery delivery = consumer.nextDelivery();
            String message = new String(delivery.getBody());
            System.out.println("Received '" + message + "'");
        }
    }
}
```
这里不得不吐槽一下，这个`Connection`和`Channel`接口并没有继承`AutoCloseable`接口，没法使用`try-with-resource`的方式进行RAII，要是java也能`duck-type`就好了，哈哈。

# play with Spring

首先新建一个`maven`工程，然后添加以下依赖:
```xml
<!-- rabbitmq相关依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

接着在配置文件中添加
```ini
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=spring
spring.rabbitmq.password=123456
```
这些配置简单易懂，看名字就知道是干啥的。

然后在代码中编写一个生产者服务，生产者服务需要注入一个`AmqpTemplate`来发送`AMQP`协议的消息。
```java
@Service
final class Sender{
	private final AmqpTemplate amqpTemplate;
	@Autowired
	Sender(AmqpTemplate amqpTemplate) {
		this.amqpTemplate = amqpTemplate;
	}

	public void send(String message){
		amqpTemplate.convertAndSend("rabbit",message);
	}
}
```

在代码中编写一个消费者模块，消费者模块需要一个`RabbitListener`注解，这个注解指明了这个消费者监听哪个队列
```java
@Component
@RabbitListener(queues = "rabbit")
final class Receiver{

	@RabbitHandler
	public void recive(String message){
		System.out.println(message);
	}
}
```

然后配置一下队列，在含有`@Configuration`注解的类中添加
```java
    @Bean
	public Queue rabbitQueue(){
		return new Queue("rabbit");
	}
```
这个`bean`增加了一个新的队列实例。不过暂时还不知道这样的注入方式有什么限制，以及rabbitMQ SDK是如何将这个队列与Listener对应起来的。这里需要去查看一下amqp-starter的实现细节。

至此，rabbitMQ的安装和简单使用都已经完成，高级内容请查看官网。

# 更多概念

在上面的试玩中，屏蔽了许多关于RabbitMQ的细节概念，这些概念对于理解RabbitMQ的配置还是比较重要的，这部分内容会比较笼统的介绍一下RabbitMQ中的概念

## AMQP

对于一个大型的软件系统来说，它会有很多的组件或者说模块或者说子系统。那么这些模块的如何通信？这和传统的IPC(Inter-Process Communication)有很大的区别。传统的IPC很多都是在单一系统上的，模块耦合性很大，不适合扩展，如果使用`socket`, 那么不同的模块的确可以部署到不同的机器上，但是还是有很多问题需要解决。比如：

 1. 信息的发送者和接收者如何维持这个连接，如果一方的连接中断，这期间的数据如何方式丢失？
 2. 如何降低发送者和接收者的耦合度？
 3. 如何让Priority高的接收者先接到数据？
 4. 如何做到负载均衡？
 5. 如何有效的将数据发送到相关的接收者？也就是说将接收者subscribe 不同的数据，如何做有效的filter。
 6. 如何做到可扩展，甚至将这个通信模块发到cluster上？
 7. 如何保证接收者接收到了完整，正确的数据？

AMDQ协议解决了以上的问题，而RabbitMQ实现了AMQP。

## 主要概念

RabbitMQ 主要包括以下几个概念：Broker、Exchange、Queue、Binding、Routingkey、Producter、Consumer、Channel

1. Broker: 简单来说就是消息队列服务器的实体。
2. Exchange: 接收消息，转发消息到绑定的队列上，指定消息按什么规则，路由到哪个队列。
3. Queue: 消息队列载体，用来存储消息，相同属性的 queue 可以重复定义，每个消息都会被投入到一个或多个队列。
4. Binding: 绑定，它的作用就是把 Exchange 和 Queue 按照路由规则绑定起来。
5. RoutingKey: 路由关键字，Exchange 根据这个关键字进行消息投递。
6. Channel: 消息通道，在客户端的每个连接里可建立多个 Channel，每个 channel 代表一个会话。

下面是这几个概念在RabbitMQ架构中的位置
![Rabbit MQ架构图](https://img-blog.csdn.net/20140220173559828)
这个系统架构图版权属于sunjun041640

接下来的内容引用自anzhsoft的[这篇文章](https://blog.csdn.net/anzhsoft/article/details/19563091),其中详细阐述了以上各个概念的含义：
> RabbitMQ Server： 也叫broker server，它不是运送食物的卡车，而是一种传输服务。原话是RabbitMQisn’t a food truck, it’s a delivery service. 他的角色就是维护一条从Producer到Consumer的路线，保证数据能够按照指定的方式进行传输。但是这个保证也不是100%的保证，但是对于普通的应用来说这已经足够了。当然对于商业系统来说，可以再做一层数据一致性的guard，就可以彻底保证系统的一致性了。

> Client A & B： 也叫Producer，数据的发送方。create messages and publish (send) them to a broker server (RabbitMQ).一个Message有两个部分：payload（有效载荷）和label（标签）。payload顾名思义就是传输的数据。label是exchange的名字或者说是一个tag，它描述了payload，而且RabbitMQ也是通过这个label来决定把这个Message发给哪个Consumer。AMQP仅仅描述了label，而RabbitMQ决定了如何使用这个label的规则。

> Client 1，2，3：也叫Consumer，数据的接收方。Consumers attach to a broker server (RabbitMQ) and subscribe to a queue。把queue比作是一个有名字的邮箱。当有Message到达某个邮箱后，RabbitMQ把它发送给它的某个订阅者即Consumer。当然可能会把同一个Message发送给很多的Consumer。在这个Message中，只有payload，label已经被删掉了。对于Consumer来说，它是不知道谁发送的这个信息的。就是协议本身不支持。但是当然了如果Producer发送的payload包含了Producer的信息就另当别论了。

> 对于一个数据从Producer到Consumer的正确传递，还有三个概念需要明确：exchanges, queues and bindings。
* Exchanges are where producers publish their messages.
* Queuesare where the messages end up and are received by consumers
* Bindings are how the messages get routed from the exchange to particular queues.

> 还有几个概念是上述图中没有标明的，那就是Connection（连接），Channel（通道，频道）。
Connection： 就是一个TCP的连接。Producer和Consumer都是通过TCP连接到RabbitMQ Server的。以后我们可以看到，程序的起始处就是建立这个TCP连接。
> Channels： 虚拟连接。它建立在上述的TCP连接中。数据流动都是在Channel中进行的。也就是说，一般情况是程序起始建立TCP连接，第二步就是建立这个Channel。