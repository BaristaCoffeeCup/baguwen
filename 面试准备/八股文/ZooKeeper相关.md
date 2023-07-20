## ZooKeeper的特点
- 高可用（构建集群）
1. 半数以上机器存活，服务就能正常运行
2. 自动进行 Leader 选举
- 顺序一致性（事务操作的顺序）
1. 每个事务请求，都会转发给 Leader 处理
2. 每个事务，会分配全局唯一的递增id（zxid，64位：epoch + 自增 id）
- 最终一致性：
1. 通过提议投票方式，保证事务提交的可靠性
2. 提议投票方式，只能保证 Client 收到事务提交成功后，半数以上节点能够看到最新数据

<br>

## ZooKeeper的应用场景
1. 数据发布订阅：
- 推: 服务端会推给注册了监控节点的客户端 Wathcer 事件通知
- 拉: 客户端获得通知后，然后主动到服务端拉取最新的数据
2. 分布式环境下配置文件的管理和同步
3. 集群管理：新节点的加入和推出，掌握各个分布式节点的状态，master节点的选举

<br>

## 请描述一下 Zookeeper 的通知机制是什么？
- 客户端注册 watcher
- 服务端处理 watcher
- 客户端回调 watcher
> Watcher是一次性的，无论是服务端还是客户端，一旦一个 Watcher 被触发，Zookeeper 都会将其从相应的存储中移除。<br>
> 客户端回调Watcher是一个串行同步的过程

<br>

## ZooKeeper中的角色
Leader（领导者）：每个ZooKeeper集群中只有一个Leader，负责处理客户端的写请求。Leader负责协调和同步所有的Follower和Observer节点，并保持集群中所有节点的数据一致性。

Follower（跟随者）：Follower节点是ZooKeeper集群中的普通节点，负责处理客户端的读请求以及将写请求转发给Leader。Follower节点通过与Leader节点保持心跳来维持与Leader的连接，并且在Leader失效时，Follower节点可以参与新的Leader选举。

Observer（观察者）：Observer节点类似于Follower节点，但不参与Leader的选举过程。Observer节点可以处理客户端的读请求，但不会参与数据的写操作。Observer节点的存在可以提高集群的读性能，减轻Leader的压力。

<br>

## ZooKeeper的整体架构
- ZooKeeper分为服务器端（Server） 和客户端（Client），客户端可以连接到整个ZooKeeper服务的任意服务器上（除非 leaderServes 参数被显式设置， leader 不允许接受客户端连接）。
- ）客户端使用并维护一个 TCP 连接，通过这个连接发送请求、接受响应、获取观察的事件以及发送心跳。如果这个 TCP 连接中断，客户端将自动尝试连接到另外的 ZooKeeper服务器。
- ZooKeeper 启动时，将从实例中选举一个 leader，Leader 负责处理数据更新等操作，一个更新操作成功的标志是当且仅当大多数Server在内存中成功修改数据。每个Server 在内存中存储了一份数据
- ）Zookeeper是可以集群复制的，集群间通过Zab协议（Zookeeper Atomic Broadcast）来保持数据的一致性；
- Zab协议包含两个阶段：leader election阶段和Atomic Brodcast阶段。
  - 集群中将选举出一个leader，其他的机器则称为follower，所有的写操作都被传送给leader，并通过brodcast将所有的更新告诉给follower
  - 当leader崩溃或者leader失去大多数的follower时，需要重新选举出一个新的leader，让所有的服务器都恢复到一个正确的状态。
  - 当leader被选举出来，且大多数服务器完成了 和leader的状态同步后，leadder election 的过程就结束了，就将会进入到Atomic brodcast的过程。
  - Atomic Brodcast同步leader和follower之间的信息，保证leader和follower具有形同的系统状态。


<br>

## ZooKeeper如何保证事务的顺序一致性？
- Zookeeper 采用了递增的事务 id 来识别，所有的 proposal （提议）都在被提出的时候加上了zxid 。
- zxid 实际上是一个 64 位数字，高 32 位是 epoch 用来标识 Leader 是否发生了改变，如果有新的 Leader出现，epoch自增， 低 32 位用来递增计数。
- 当新产生 proposal 的时候，会向其他的 Server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

<br>

## ZooKeeper的选举流程
- 检测Leader失效：当ZooKeeper集群中的节点无法与当前的Leader节点保持心跳连接时，节点会认为Leader节点失效，触发新的Leader选举过程。

- 节点状态变化：集群中的节点将自己的状态从Follower或Observer更改为LOOKING状态，表示它们正在寻找新的Leader节点。

- 发起提议：LOOKING状态的节点会发起提议，通过向其他节点发送投票请求来参与选举。每个节点会为自己投票，并将自己的选票广播给其他节点。

- 统计投票：每个节点在接收到投票请求后，会比较候选Leader节点的ZXID（操作编号）和自己本地保存的ZXID，选择ZXID较大的节点作为候选Leader，当ZXID相同时比较SID，即节点ID，选取较大者，比较结束后节点会将本地存储的选票修改为胜出的那一个节点，并广播出去。节点将收到的投票结果统计起来。

- 选举结果：当某个节点收到大多数节点的投票后，它将成为新的Leader节点，并向其他节点广播自己的选举结果。其他节点接收到选举结果后，将更新自己的Leader信息。

- 同步数据：新选举的Leader节点会通知其他节点同步数据，确保集群中所有节点的数据一致性











