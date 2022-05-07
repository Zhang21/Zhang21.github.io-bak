# kafka


参考:

- docs: <https://kafka.apache.org/documentation>

<br/>

版本:

- kafka: v3.0





<br/>
<br/>

<!--more-->

<br/>
<br/>






# 介绍

Kafka是一个分布式流式平台。





<br/>

---

<br/>





# 术语

kafka使用中的一些标准词汇。

- Producer(生产者): 消息的发布者
- Consumer(消费者): 消息的使用者
- Broker(节点): 集群包含多个节点
- Replication(备份机制): 每个节点有相同的副本
- Replica(副本)
  - Leader Replica
  - Follower Replica
- Topic(主题): 每个主题配置M个分区
- Partitioning(分区): 一组有序的消息，从0开始。每个分区配置N个副本，1个领导者副本，N-1个追随者副本。
- Segment(段)
- Record(消息): 每条消息包含键、值和时间戳
- Offset(偏移): 分区中的每条消息都会分配一个有序的id，从0开始
- Consumer Group(消费者组)
- Consumer Offset(偏移): 记录每个消费者当前的消费位置







































