常见命令
===

Author: 郑文峰

一些组件的常用命令。

kafka
-----

### topic操作

<!--rehype:wrap-class=col-span-2-->

```shell
./kafka-topics.sh --zookeeper $zookeeper_ip:$zookeeper_port --create --topic $topic --replication-factor $replication_factor --partitions $partitions
```

<!--rehype:className=wrap-text -->

创建topic

```shell
kafka-topics.sh --list --zookeeper $zookeeper_ip:$zookeeper_port
```

获取所有的topic

### topic操作参数

参数 | 作用
:- | --
--replication-factor | 副本数
--partitions | partitions数
--create | 创建topic
--delete | 删除topic

### 消费topic

<!--rehype:wrap-class=col-span-2-->

```shell
kafka-console-consumer.sh --group $group --bootstrap-server $kafka_host:$kafka_port --topic $topic
```

使用消费者组group对topic进行消费

```shell
kafka-console-consumer.sh --consumer.config /tmp/client.properties --group $group --bootstrap-server $kafka_host:$kafka_port --topic $topic
```

<!--rehype:className=wrap-text -->

以认证的方式进行消费

```properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=SCRAM-SHA-256
```

client.properties 配置文件

### 消费topic参数

<!--rehype:wrap-class=col-span-1-->

参数 | 作用
:- | --
--group | 消费者组名称
--bootstrap-server | kafka broker的地址
--topic | 需要消费的topic
--from-beginning | 从开头开始消费

### 生产topic

<!--rehype:wrap-class=col-span-3-->
```shell
kafka-console-producer.sh --broker-list $host:$port --topic $topic
```

对topic进行生产数据. 该方式为未认证方式

```shell
kafka-console-producer.sh --producer.config /tmp/client.properties --broker-list $host:$port --topic $topic
```

以认证的方式连接kafka并生产数据.

### acl操作
<!--rehype:wrap-class=col-span-2-->

```shell
kafka-acls.sh --authorizer-properties zookeeper.connect=$zookeeper_host:$zookeeper_port --add --allow-principal User:$username --allow-host IP:$ip --topic $topic
```
<!--rehype:className=wrap-text -->

添加对topic的acl限制，仅允许username和来源ip访问

```shell
kafka-acls.sh --authorizer-properties zookeeper.connect=$zookeeper_host:$zookeeper_port --list
```

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

```shell
kafka-consumer-groups.sh--bootstrap-server $kafka_host:$kafka_port --list
```

查看所有消费者组

### 认证配置
<!--rehype:wrap-class=col-span-2-->

* 创建kafka_jaas.conf

    ```
    KafkaClient {
        org.apache.kafka.common.security.scram.ScramLoginModule required
        username="kafka_user"
        password="kafka_password";
    };
    ```

* 创建client.properties

    ```
    Client {
        org.apache.kafka.common.security.scram.ScramLoginModule required
        username="zookeeperUser"
        password="zookeeperPassword";
    };
    ```

* 创建环境变量，指定kafka_jaas.conf文件的绝对路径

    ```shell
    export KAFKA_OPTS="-Djava.security.auth.login.config=/tmp/kafka_jaas.conf"
    ```

<!--rehype:className=style-timeline-->

#### 认证的方式使用kafka命令

消费

```shell
kafka-console-consumer.sh --consumer.config ./client.properties --bootstrap-server ${kafka_server}:9092 --topic ${topic}
```
<!--rehype:className=wrap-text -->

生产

```shell
kafka-console-producer.sh --producer.config ./client.properties --broker-list ${kafka_server}:9092 --topic ${topic}
```
<!--rehype:className=wrap-text -->

其他命令

```shell
kafka-consumer-groups.sh  --command-config ./client.properties --bootstrap-server ${kafka_server}:9092 --describe --group ${group_id}
```
<!--rehype:className=wrap-text -->

helm
-----

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

pulsar
---

### pulsar-admin
<!--rehype:wrap-class=col-span-3-->

查看租户

```shell
./pulsar-admin --admin-url http://${pulsar_url}:${pulsar_admin_port} tenants
```

创建租户

```shell
./pulsar-admin --admin-url http://${pulsar_url}:${pulsar_admin_port} tenants create ${tenant_name}
```

查看namespace

```shell
./pulsar-admin --admin-url http://${pulsar_url}:${pulsar_admin_port} namespaces 
```

创建namespace

```shell
./pulsar-admin --admin-url http://${pulsar_url}:${pulsar_admin_port} namespaces create ${tenant_name}/${namespace_name}
```

创建topic

```shell
./pulsar-admin --admin-url http://${pulsar_url}:${pulsar_admin_port} topics create persistent://${tenant_name}/${tenant_name}/${topic_name}
```

### pulsar-admin 查看topic中的详情
<!--rehype:wrap-class=col-span-2-->
```shell
./pulsar-admin topics partitioned-stats persistent://${tenant_name}/${tenant_name}/${topic_name} --per-partition
```

### pulsar-admin 查看topic中的详情说明
<!--rehype:wrap-class=col-span-1-->

参数 | 作用
:- | --
msgRateIn | 生产的速率
msgRateOut | 被消费的速率
subscriptions | 该topic的订阅者
msgBacklog | 订阅者堆积数
partitions | partitions数

### pulsar-client 生产
<!--rehype:wrap-class=col-span-2-->

```shell
./pulsar-client pulsar://${pulsar_url}:${pulsar_port} produce -m "${message}" persistent://${tenant_name}/${tenant_name}/${topic_name} 
```
<!--rehype:className=wrap-text -->

### 生产的参数
<!--rehype:wrap-class=col-span-1-->
参数 | 作用
:- | --
-m | 生产的消息

### pulsar-client 消费
<!--rehype:wrap-class=col-span-2-->

```shell
./pulsar-client pulsar://${pulsar_url}:${pulsar_port} consume -s "${consumer_name}" persistent://${tenant_name}/${tenant_name}/${topic_name} -n 0
```
<!--rehype:className=wrap-text -->

### 消费的参数
<!--rehype:wrap-class=col-span-1-->
参数 | 作用
:- | --
-s | 消费者名称
-n | 永不停止的消费
