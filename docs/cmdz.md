常见命令
===

Author: 郑文峰

一些组件的常用命令。

kafka
-----

### 创建topic

<!--rehype:wrap-class=col-span-3-->
```shell
./kafka-topics.sh --zookeeper ip:host --create --topic demo-topic  --replication-factor 3 --partitions 2
```

* replication-factor: 副本数

### 获取所有的topic

```shell
kafka-topics.sh --list --zookeeper ip:port
```

### 消费topic

<!--rehype:wrap-class=col-span-2-->
```shell
kafka-console-consumer.sh --group zwf --bootstrap-server kafka:9092 --topic topic_a
```

* --group：消费者组名称
* --bootstrap-server：kafka broker的地址
* --topic：需要消费的topic
* --from-beginning：从开头开始消费

### 生产topic

<!--rehype:wrap-class=col-span-2-->
```shell
./kafka-console-producer.sh --broker-list kafka-host(host:port)--topic demo-topic
```

这里的kafka-host可以是多个broker地址

helm
-----

### 安装helm包

<!--rehype:wrap-class=col-span-2-->

```shell
helm install helm名称 helm包名称 --version="helm包版本号" -n namespace -f values.yaml
```

* --version: helm包版本号
* -n: namespace
* -f: 参数文件
