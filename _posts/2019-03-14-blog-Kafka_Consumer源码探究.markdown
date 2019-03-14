---
layout: default
title:  "Kafka Consumer源码探究"
date:   2019-03-14 15:22:00
categories: main
---

# Kafka Consumer 源码探究
## 1. **该类的前言注释阅读**
- Kafka Consumer类不是线程安全的
- 从0.10.1版本才开始支持offsetsForTimes方法
- 消费位点offset从0开始
- 提交分为自动提交和手动提交（同步提交和异步提交）
- consumer和partition的关系为1对多，一个consumer能同时消费多个分区，但一个分区同一时刻只能被一个consumer消费，保证了分区内数据有序
- 当一个consumer group中有consumer挂掉或者加入了新的consumer时会发生rebalance
- 虽然consumer有心跳检测机制来检测consumer是否live，但是如果一个consumer可以发送心跳，但它的进程没了，这种情况心跳检测机制无法解决，所以还有一种“poll检测机制”，如果一个consumer超过一段时间内没有进行poll操作了，那么kafka就会认为该consumer挂掉了，进而把它踢出consumer group。以下两个参数用于控制poll操作的间隔时长：
**_TIPS:_ 所以在需要长时间消费的场景下，请把poll操作放在while循环体内**
    1. _max.poll.interval.ms_ ：最大poll间隔时长
    2. _max.poll.records_ ：最大poll记录数（达到多大的数据量才poll）
    例：
    `Properties props = new Properties();
    props.put("bootstrap.servers", "localhost:9092");
    props.put("group.id", "test");
    props.put("enable.auto.commit", "true");
    props.put("auto.commit.interval.ms", "1000");
    props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
    props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
    KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
    consumer.subscribe(Arrays.asList("foo", "bar"));
    while (true) {
        ConsumerRecords<String, String> records = consumer.poll(100);
        for (ConsumerRecord<String, String> record : records)
            System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), 
            record.value());
    }`
- consumer的subscribe方法只能指定topic，而assign方法则可以细粒度得指定到partition 
_TIPS:_ **subscribe会触发rebalance，但是assign不会触发rebalance，所以当要保证assign时不会发生冲突，请保证每个consumer的group id不同**
- 要想精确得做到“exactly once”，就要保证保存计算的结果和提交offset这两个操作是原子性的
- 要想监听到consumer的assign和rebalance操作，可以实现ConsumerRebalanceListener里对应的方法。通过`consumer.subscribe(Collection, ConsumerRebalanceListener)`实现。
- 通过seek方法可以指定partition的消费位点
- consumer可以动态得控制每个partition的消费速度
- 从0.11.0版本开始，kafka开始支持事务。 [相关博客](https://www.cnblogs.com/devos/p/9562993.html) kafka通过以下三个方面支持事务：
    1. **LSO last stable offset**
    2. **Aborted Transaction Index**  这涉及到FetchResponse的消息格式的变化，在FetchResponse里包含了其中每个TopicPartition的记录里的aborted transactions的信息，consumer使用这些信息，可以更高效地从FetchResponse里包含的消息里过滤掉被abort的消息。
    3. **Consumer端根据aborted transactions的消息过滤**

    **通过对aborted transaction index和LSO的使用，Kafka使得consumer端可以高效地过滤掉aborted transaction里的消息，从而减小了事务机制的性能开销。**
- 多线程处理，官方给出的两点建议：
    1. **一个线程一个Consumer**
    2. **把消费和处理数据解耦**

## 2. **Kafka Consumer类的源码阅读**

-  **assignment()**
获得最近assign的分区集合

***

- **subscription()**
获得最近subscribe的topic集合

***

- **subscribe(Collection<String> topics, ConsumerRebalanceListener listener)**
消费指定topic的数据
    - _topics:_ 要消费的topic
    - _listener:_ consumer发生rebalance时的监听器
_**当发生如下几种情形时，触发rebalance：**_
    1. 消费的topic的分区数发生了变化
    2. 新增或销毁了消费的topic
    3. consumer group 里的成员宕机或者失败
    4. 有新的成员加入了consumer group

现在我们来理解一下源码：

    1. acquireAndEnsureOpen();
尝试给当前Consumer加锁，如果加不了锁就报错（因为Consumer是线程不安全的类）

***

    2. 判断topics是否为空
topic参数不能为空，为空就报错

***

    3. throwIfNoAssignorsConfigured()
Consumer必须配置消费分区策略，否则报错

***

    4. fetcher.clearBufferedDataForUnassignedTopics(topics)
    5. this.subscriptions.subscribe(new HashSet<>(topics), listener)
    6. metadata.setTopics(subscriptions.groupSubscription())

***



