常见命令
===

Python的第三方库使用Demo

组件
-----

### 创建topic

```shell
./kafka-topics.sh --zookeeper ip:host --create --topic demo-topic  --replication-factor 3 --partitions 2
```

* replication-factor: 副本数

### 消费topic

```shell
kafka-console-consumer.sh --consumer zwf --bootstrap-server kafka:9092 --topic topic_a
```

* --consumer：消费者名称
* --bootstrap-server：kafka broker的地址
* --topic：需要消费的topic
* --from-beginning：从开头开始消费

### 生产topic

```shell
./kafka-console-producer.sh --broker-list kafka-host(host:port)--topic demo-topic
```

### 获取所有的topic

```shell
kafka-topics.sh --list --zookeeper ip:port
```

