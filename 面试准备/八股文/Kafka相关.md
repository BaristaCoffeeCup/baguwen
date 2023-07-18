
## Kafka系统组成部分及各部分简单介绍
1. broker： Kafka 服务器，负责消息存储和转发
2. topic：消息类别， Kafka 按照 topic 来分类消息
3. partition： topic 的分区，一个 topic 可以包含多个 partition， topic 消息保存在各个partition 上
4. offset：消息在日志中的位置，可以理解是消息在 partition 上的偏移量，也是代表该消息的唯一序号
5. Producer：消息生产者
6. Consumer：消息消费者
7. Consumer Group：消费者分组，每个 Consumer 必须属于一个 group
8. Zookeeper：保存着集群 broker、 topic、 partition 等 meta 数据；另外，还负责 broker 故障发现， partition leader 选举，负载均衡等功能

> 一个topic分为多个partition，每个partition对应于一个log文件，为防止log文件过大导致数据定位效率低下，Kafka采取了分片和索引机制，每个partition分为多个segment。每个segment包括：“.index”文件、“.log”文件和.timeindex等文件，Producer生产的数据会被不断追加到该log文件末端。

<br>

## Partition中的Message的组成
每条Message由以下三部分组成
- offset: Message在当前Partition中的偏移量，不指代实际的存储位置，仅指明唯一一条信息
- MessageSize: data的数据大小
- data: 实际数据

<br>

## Kafka的持久化机制
- Kafka 的持久化是通过将消息写入磁盘上的日志文件来实现的。具体而言，Kafka 使用了两种类型的日志文件：日志段（log segment）和索引文件（index file）。

- 每个主题（topic）在 Kafka 中都有一个或多个分区（partition），每个分区由一系列连续的日志段组成。当生产者发送消息到 Kafka 时，消息首先被追加到当前活跃的日志段中。一旦日志段达到一定的大小限制（通过配置参数控制），Kafka 将会创建一个新的日志段，并将新的消息追加到其中。这样，Kafka 就实现了消息的持久化存储。

- 为了提高读取效率，Kafka 还使用了索引文件。索引文件包含了每个日志段中消息的偏移量（offset）和物理位置的索引信息。当消费者需要读取消息时，它可以根据索引文件快速定位到指定偏移量的消息所在的物理位置，从而实现高效的读取操作。

- 此外，Kafka 还支持数据的复制和备份，以增加数据的可靠性。每个分区都可以配置多个副本（replica），这些副本分布在不同的 Kafka 节点上。当消息被写入主分区时，Kafka 会将消息复制到其他副本中，以确保数据的冗余存储。如果主分区发生故障，副本可以接管并继续提供服务。

<br>

## Kafka的主从同步
- 每一个topic包含多个partition，每一个partition允许有多个分区
- 创建副本的单位是topic的分区，每个分区都有一个leader和零或多个followers.
- 所有的读写操作都由leader处理，一般分区的数量都比broker的数量多的多，各分区的leader均匀的分布在brokers中。
- 所有的followers都复制leader的日志，日志中的消息和顺序都和leader中的一致。flowers向普通的consumer那样从leader那里拉取消息并保存在自己的日志文件中。
  
<br>

## 如何判断一个Kafka的分区副本节点还处于存活状态
- 节点必须可以维护和ZooKeeper的连接，Zookeeper通过心跳机制检查每个节点的连接。
- 如果节点是个follower,他必须能及时的同步leader的写操作，延时不能太久。

<br>

## Kafka中的ISR、AR、OSR
- ISR:In-Sync Replicas 副本同步队列
- OSR:Outof-Sync Replicas， follower从leader同步数据有一些延迟，任意一个超过阈值都会把follower剔除出ISR, 存入OSR列表
- AR:Assigned Replicas 所有副本
> ISR中的每一个副本节点都可以被选举为leader节点

<br>

## Kafka中ack的不同值指代了什么含义？
- 1（默认） 数据发送到Kafka后，经过leader成功接收消息的的确认，就算是发送成功了。在这种情况下，如果leader宕机了，则会丢失数据。
- 0 生产者将数据发送出去就不管了，不去等待任何返回。这种情况下数据传输效率最高，但是数据可靠性确是最低的。
- -1 producer需要等待ISR中的所有follower都确认接收到数据后才算一次发送完成，可靠性最高。当ISR中所有Replica都向Leader发送ACK时，leader才commit，这时候producer才能认为一个请求中的消息都commit了。


<br>

## 如何避免Kafka重复消费
Kafka通过消费者组和偏移量的管理来避免重复消费。消费者组确保每个分区只能由一个消费者进行消费，而偏移量跟踪每个消费者在分区中的位置，以便在故障或离线情况下恢复消费进度。

<br>

## Kafka 如何保证消息消费的顺序性？
