# kafka篇

## kafka组成与原理









##  常用指令

##### 启动关闭

启动在各自节点上执行  kafka-server-start.sh -daemon $KAFAKA_HOME/config/server.properties

关闭:  各自节点执行 kafka-sever-stop.sh

##### 1.查看topic

kafka-topics.sh  --bootstrap-server   lpc@project1:9092  --list 





##### 2.创建topic

kafka-topics.sh   --bootstrap-server  lpc@project1:9092  \
 --create --topic name   --partitions 2   --replication-factor 2

##### 3.删除topic

kafka-topics.sh  --bootstrap-server  lpc@project1:9092 \

--delete --topic  name

需要server.properties中设置delete.topic.enable=true否则只是标记删除。

##### 4.查看topic详情

kafka-topics.sh --bootstrap-server dev@pre-13:9092 --describe --topic first

##### 5.修改分区数

kafka-topics.sh --bootstrap-server dev@pre-13:9092 /kafka --alter --topic first --partitions 6

和topic相关的用bootstrap-server服务端

--broker-list是broker的节点。



##### 向topic中发数据（必须是broker_list）

用bootstrap_server不行，报错

kafka-console-producer.sh   --broker-list     lpc@project1:9092   --topic   flink_word_topic



##### 消费topic数据

kafka-console-consumer.sh --bootstrap-server dev@pre-13:9092 --from-beginning  --topic aaa

/usr/local/kafka/kafka/bin/kafka-console-consumer.sh --bootstrap-server dev@pre-13:9092 --topic user_behavior



./kafka-console-consumer.sh --bootstrap-server  datacollkafka1:9092     --topic   sa_event_topic



## API消费kafka事项

#### 消费者偏移量：

kafka对有消费者组偏移量有3中，latest,earliest,none  默认是latest，可以指定是earliest	

kafka自己的API种，kafka的auto.reset.offsets
earlist:如果有offset,从offset继续消费，没有从最早消费
lastest:如果有offset,从offset继续消费，没有从最新消费

并且kafka可以代码中实现指定偏移量消费，比如一天前的消息，需要重新消费一下。这个要去kafka api去观看

# 云服务器kafka配置

就是如果你没有做hosts映射的话，你kafka配置文件需要advertised.listeners=PLAINTEXT://x.x.x.x:9092

注意不要改#listeners=PLAINTEXT:x.x.x.x//:9092，这个改了之后kafka启动不起来。