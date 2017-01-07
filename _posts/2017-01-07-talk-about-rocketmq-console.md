## 什么是rocketmq-console-ng

rocketmq-console-ng是为[incubator-rocketmq](https://github.com/apache/incubator-rocketmq)量身打造的一个web控制台。通过它我们可以对rocketMQ进行状态查看，配置变更，管理操作各个节点。

## 为什么要有rocketmq-console-ng
一个系统在线上运行，如果我们无法知道它的状况，进行相应的控制，那是十分危险的。无法知道运行状况的系统就好比一个人被麻醉了的人，流了很多血，却无法自知自救，很快就会go die。

系统在追求高性能、高可用的同时，复杂性也会提高，RocketMQ是一个相对复杂的消息中间件，对它的管理也尤为重要。目前rocketMq已经提供了一个功能相对齐全的[CLI Admin Tool](https://github.com/alibaba/RocketMQ/wiki/CLI-Admin-Tool),让我们进行rocketMQ的查看与控制。命令行的弊端就是不容易上手，越是复杂的系统，指令越是繁多。也因此rocketMq也有一个可视化的[rocketmq-console](https://github.com/rocketmq/rocketmq-console),它对一些Cli Admin Tool的方法进行了封装与发展，提供了可视化的操作界面，更加方便了我们对rocketMQ的管理。

#### 我们的目标是在这个的基础上，走的更远。

#### 在不久后的将来，你将会用到一个功能更强大，易用性更高的控制台。

## 那么问题来了，什么样的console是个好console？

## 需要考虑的点
* 1.国际化
能让更多的人很方便的用起来

* 2.产品的概念
不仅仅是个工具，它应该是个完善的产品。使用rocketMQ的人几乎都会使用它，那么功能更强大，交互更方便、灵活，易用性高，人性化是十分必要的。说白了就是要好用

* 3.UI美观
好的UI让人赏心悦目，心情舒畅。

* 4.技术选型
技术更加通用，而不是小众的技术。这样有利于大家一起维护。

* 5.代码质量
谁都喜欢阅读以及完善干净的代码，需要有一个好的设计，保证代码的质量

## 一个rocketMQ-console需要具备的点
从功能上入手，从产品使用者的层面进行分析。

* 作为业务开发人员：

可能更多的关注自己发送/接受消息的投递/消费情况。我的消息发成功了吗？都有谁消费？能发个测试消息吗？为什么消费失败了。。等等

* 作为rocketMQ的管理员：

可能更多的关注我们有哪些节点，各个broker现在状态如何？投递消息有多少延迟？哪些消息积压了？。。等等

### 那么从使用习惯和功能划分上看，可以分为以下一些模块：

#### Cluster管理
* 1.集群的节点分布（cluster-broker(master/slave)）
* 2.每一个broker节点的运行状态
* 3.每一个broker节点的配置信息

#### Topic管理
* 1.Topic列表（分页/筛选）
* 2.Topic状态及路由 （对应的broker/queue/offset）
* 3.这个Topic对应的消费组及消费状态
* 4.Topic的配置查看
* 5.Topic的添加与修改（修改基于配置查看）
* 6.手动发送一个此主题的自定义消息
* 7.重置次消息消费位点（不管consumer在线与否）
* 8.删除这个topic（删除哪些数据，何时生效）

#### Producer管理
* 1.Producer列表（分页/筛选）(目前似乎不好做)
* 2.Producer的client版本信息以及host:port

#### Consumer管理
* 1.Consumer列表（分页/筛选）
* 2.Consumer的Client的详细信息
* 3.此Consumer下所有Topic的消费明细
* 4.Consumer的配置查看
* 5.Consumer的添加与修改（修改基于配置查看）
* 6.删除这个Consumer（删除哪些数据，何时生效）

#### Message管理
* 1.按照时间区间和Topic查询
* 2.按照Topic和Key查询
* 3.按照MessageId（offsetMessageId）查询
* 4.Message查看界面友好信息全
* 5.对应的Message的消费情况（成功/为何失败）
* 6.往这个Message对应的指定Group再次发送消息

#### Config管理(这个很少用,我也不熟,是否需要？)（TO BE DONE）
* 1.BrokerConfig
* 2.NamesvrKvConfig


### 如何开发rocketmq-console-ng
我们目前是基于rocketmq-tools这个jar进行开发的，里面的com.alibaba.rocketmq.tools.admin。MQAdminExt提供了大量console需要的功能，我们可以现在这个的基础下进行优化。等第一版有了一个雏形并且稳定之后，再考虑拓展。（毕竟console的数据来源于namesvr/broker/consumer,定制化需求需要暴露特定的方法，改动较大）

    <dependency>
        <groupId>com.alibaba.rocketmq</groupId>
        <artifactId>rocketmq-tools</artifactId>
        <version>${rocketmq.version}</version>
    </dependency>
    
### 技术选型
（一个Java版本的console 个人感觉spring-boot+bootstrap+angularjs还是不错的）

大家一起商量吧~

### 问题点
最近也有一些朋友提出了一些目前console 或者说Admin Cli的问题（需求），看起来也是需要权衡和完善的。

比如：

* 删除了Topic但是还是会被消费
* 想看单纯的rocketMQ的硬盘占用情况

##### 我理解应该是还有很多需求和建议的，希望大家多多反馈了。只有这样产品才会越来越好。

## 总结
那么综上所述，我们能有一个大致的RoadMap了

# Roadmap

## Producer
- [ ] ClusterController
    - [ ] Cluster OverView
    - [ ] Broker Status
    - [ ] Broker Config

## Topic
- [ ] TopicController
    - [ ] TopicList
    - [ ] Topic Status
    - [ ] Topic Router
    - [ ] View Topic Config
    - [ ] Topci Add / Update
    - [ ] Send A Test Topic
    - [ ] Reset ConsumerGroup's Offset Under This Topic
    - [ ] Delete This Topic

## Producer
- [ ] ProducerController
    - [ ] ProducerList
    - [ ] Producer Client Info


## Consumer
- [ ] ConsumerController
    - [ ] ConsumerList
    - [ ] Consumer Client Info
    - [ ] Topic Consume Status Under This Consumer Group
    - [ ] View Consumer Config
    - [ ] Consumer Add / Update
    - [ ] Delete This Consumer

## Message
- [ ] MessageController
    - [ ] Query By Topic And Time
    - [ ] Query By Topic And Key
    - [ ] Query By MessageId(OffsetMessageId)
    - [ ] A Nice Message Detail View
    - [ ] Message Consume Status
    - [ ] Resend Message To A Consume Group

## Config (todo disscuss is it necessary)
- [ ] ConfigController
    - [ ] BrokerConfig
    - [ ] NamesvrKvConfig


