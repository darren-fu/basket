
# 接受消息
转发消息到绑定的队列。四种类型：direct, topic, headers and fanout
- direct：转发消息到routigKey指定的队列
- topic：按规则转发消息（最灵活）
- headers：（这个还没有接触到）
- fanout：转发消息到所有绑定队列 
# message 
消息有14个属性，最常用的几种：
- deliveryMode：持久化属性
- contentType：编码
- replyTo：指定一个回调队列
- correlationId：消息id
- mandatory： 是否在消息被发送到交换器并且还未投递到队列（没有绑定器存在）的时候得到通知
- immediate：是否在消息没有消费者能够立即处理的时候得到通知
- persistent server将会尝试将这些消息存储在一个稳定的位置，直到server崩溃。当然，这些消息肯定不会被投递到非持久的队列中
# queue
队列的属性：
- 持久性：如果启用，队列将会在server重启前都有效。
- 自动删除：如果启用，那么队列将会在所有的消费者停止使用之后自动删除掉自身。
- 惰性：如果没有声明队列，那么在执行到使用的时候会导致异常，并不会主动声明。
- 排他性：如果启用，队列只能被声明它的消费者使用。

# exchange
exchange，binding在各个机器是共享的
交换器的属性：
- 持久性：如果启用，交换器将会在server重启前都有效。
- 自动删除：如果启用，那么交换器将会在其绑定的队列都被删除掉之后自动删除掉自身。
- 惰性：如果没有声明交换器，那么在执行到使用的时候会导致异常，并不会主动声明。
 
# 高可用
- 消息ACK
- 消息和队列的持久化
- publisher confirms
- master/slave机制，配合Mirrored Queue

# 镜像队列
```shell
rabbitmqctl set_policy ha-all "^ha." ‘{"ha-mode":"all"}‘
rabbitmqctl set_policy ha-two "^two\." ^
   "{""ha-mode"":""exactly"",""ha-params"":2,"ha-sync-mode":"automatic"}"
```
 
# 分布式的RabbitMQ
RabbitMQ提供了3中分布式的解决方案，cluster，federation，shovel。cluster用于可靠的本地局域网，后两种用于不可靠的网络。
- Cluster 
同一个数据中心
- federation 
WAN广域网，允许一个exchange从另外一台机器或者cluster的exchange中接收消息，因为是两个exchange联合起来，所以必须有相同的用户权限。联合起来的exchange是单向的点对点的连接。 
- shovel 
从一边的queue中取走消息并发送到另一个exchange。通常在通过internet连接broker的时，并且需要获得比Federation更多控制权的时候使用Shovel。
