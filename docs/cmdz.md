常见命令
===

Author: 郑文峰

一些组件的常用命令。

kafka
-----

### topic参数
<!--rehype:wrap-class=col-span-1-->

参数 | 作用
:- | --
--replication-factor | 副本数
--partitions | partitions数
--create | 创建topic
--delete | 删除topic

### topic操作

<!--rehype:wrap-class=col-span-3-->
```shell
./kafka-topics.sh --zookeeper $zookeeper_ip:$zookeeper_port --create --topic $topic --replication-factor $replication_factor --partitions $partitions
```

创建topic

```shell
kafka-topics.sh --list --zookeeper $zookeeper_ip:$zookeeper_port
```

获取所有的topic

### 消费topic参数
<!--rehype:wrap-class=col-span-1-->

参数 | 作用
:- | --
--group | 消费者组名称
--bootstrap-server | kafka broker的地址
--topic | 需要消费的topic
--from-beginning | 从开头开始消费

### 消费topic

<!--rehype:wrap-class=col-span-3-->
```shell
kafka-console-consumer.sh --group $group --bootstrap-server $kafka_host:$kafka_port --topic $topic
```

使用消费者组group对topic进行消费

```shell
kafka-console-consumer.sh --consumer.config /tmp/client.properties --group $group --bootstrap-server $kafka_host:$kafka_port --topic $topic
```

以认证的方式进行消费

```properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=SCRAM-SHA-256
```

client.properties 配置文件

### 生产topic

<!--rehype:wrap-class=col-span-2-->
```shell
kafka-console-producer.sh --broker-list $host:$port --topic $topic
```

对topic进行生产数据. 该方式为未认证方式

```shell
kafka-console-producer.sh --producer.config /tmp/client.properties --broker-list $host:$port --topic $topic
```

以认证的方式连接kafka并生产数据.

### acl参数
<!--rehype:wrap-class=col-span-1-->

参数 | 作用
:- | --
--allow-principal | 允许访问的用户.
--allow-host | 允许访问的ip
--add | 添加acl
--remove | 删除acl
--list | 查看acl
--topic | 指定topic进行acl限制

### acl操作
<!--rehype:wrap-class=col-span-3-->

```shell
kafka-acls.sh --authorizer-properties zookeeper.connect=$zookeeper_host:$zookeeper_port --add --allow-principal User:$username --allow-host IP:$ip --topic $topic
```

添加对topic的acl限制，仅允许username和来源ip访问

```shell
kafka-acls.sh --authorizer-properties zookeeper.connect=$zookeeper_host:$zookeeper_port --list
```

查看acl规则列表

### 消费者组操作
<!--rehype:wrap-class=col-span-3-->

```shell
kafka-consumer-groups.sh  --bootstrap-server $kafka_host:$kafka_port --describe --group $group
```

可以查看消费者消费topic的情况，包含当前的offset和topic的最新offset等

```shell
kafka-consumer-groups.sh--bootstrap-server $kafka_host:$kafka_port --delete --group $group
```

删除消费者组

helm
-----

<!--rehype:wrap-class=col-span-2-->

### 命令

命令 | 作用
:- | --
install | 安装helm
upgrade | 更新helm，其参数基本与install相同

### 命令参数

<!--rehype:wrap-class=col-span-2-->

参数 | 作用
:- | --
--version | helm包版本号
-n | namespace
-f | 参数文件
 --dry-run | 不会真正执行操作
 --debug | debug模式

### 安装helm包

<!--rehype:wrap-class=col-span-3-->

```shell
helm install helm名称 helm包名称 --version="helm包版本号" -n namespace -f values.yaml
```

指定namespace使用values.yaml作为参数安装helm

```shell
helm install --dry-run --debug helm名称 helm包名称 --version="helm包版本号" -n namespace -f values.yaml
```

通过添加`--dry-run --debug`可以对安装helm进行测试验证其中的语法和配置。
