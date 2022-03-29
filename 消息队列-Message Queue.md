## 消息队列概念
1. 消息队列（Message Queue）是保存消息的一个容器，本质是一个队列
	- [1] 消息 Message 是指在应用之间传递的数据，消息可以是文本字符串也可以更复杂
	- [2] 消息队列 Message Queue 是一种应用间的通信方式，消息发送后可以立即返回，有消息系统来确保信息的可传递性
	- [3] Producer：消息生产者，负责产生和发送消息到Broker
	- [4] Queue: A buffer that stores messages
	- [5] Broker: 消息处理中心。负责消息存储，确认，重试等，一般其中会包含多个queue
	- [6] Consumer: 消息消费者，负责从Broker中获取消息，并进行相应处理
	  
	  **The core idea in the messaging model in RabbitMQ is that the producer never sends any messages directly to a queue. Instead, the producer can only send messages to an exchange** [illustration](https://www.rabbitmq.com/tutorials/tutorial-three-python.html)
	  
	  [message_queue](/images/MQ_sample.png)
## 消息队列的作用
	- 异步：消息堆积能力；发送方接收方不需要同时在线
	- 解耦： 防止引入过多的API给系统的稳定性带来风险
	- 削峰： 发送方和接收方不需同时在线，发送方接收不需要同时扩容
	- 提供路由： 发送方无需与接收方建立连接，双方通过消息队列保证消息能够从发送者路由到接收者，甚至对于本来网络不易互通的两个服务，也可以提供消息路由
## 常见消息队列对比
 ![Message_Queue_tables](/images/MQ_tables.png)
## 发布订阅模式
 ![发布订阅模式](/images/publish_subscribe.png)

1. 介绍 Introduction
发布/订阅模式(Publish Subscribe Pattern)属于设计模式行为中的Behavior Patterns。在软件结架构中发布/订阅是一种消息范式，消息的发送者(发布者Publisher)不会将消息直接发送给接收者(订阅者Subscriber)，而是通过消息通道广播出去，让订阅消息主题的订阅者消费到。
2. 优点
	- [1] 松耦合 Independence
	- [2] 高伸缩性 Scalability
	- [3] 高可靠性 Reliability
	- [4] 灵活性 Flexibility
	- [5] 可测试性
- 3. 应用场景
	- 应用程序需要向大量消费者广播信息。例如微信订阅号就是一个消费者量庞大的广播平台。
	- 应用程序需要与一个或多个独立开发的应用程序或服务通信，这些应用程序或服务可能使用不同的平台、编程语言和通信协议。
	- 应用程序可以向消费者发送信息，而不需要消费者的实时响应。被集成的系统被设计为支持其数据的最终一致性模型。
	- 应用程序需要将信息传递给多个消费者，这些消费者可能具有与发送者不同的可用性要求或正常运行时间计划。例如你消息在上午发布了出去，消费者计划在下午才去处理这些消息。
-