# Raft分布式一致性协议

## summary

```text
Raft is a consensus algorithm for managing a replicated log. It produces a result equivalent to (multi-)Paxos, and it is as efficient as Paxos, but its structure is different from Paxos; this makes Raft more understandable than Paxos and also provides a better foundation for building practical systems.
--《In Search of an Understandable Consensus Algorithm》
```

- 强一致性（线性一致性）：并不是指集群中所有节点在任一时刻的状态必须完全一致，而是指一个目标，即让一个分布式系统看起来只有一个数据副本，并且读写操作都是原子性的，这样应用层就可以忽略系统底层多个数据副本之间的同步问题。
- 共识算法（consensus algorithm）：共识算法可以保证即使在小部分（x <= (n-1)/2）节点故障的情况下，系统任然能正常对外提供服务。共识算法通常基于复制机（Replicated State Machine）模型，也就是所有节点从同一个state出发，经过同样的操作log，最终达到一致的state。

## Raft的简介

Raft使用Quorum机制来实现共识和容错，如果将对raft集群的操作是提案，每当发起一个天，必须得到大多数（x > n/2）节点的统一才能提交。

- **'提案'：在上下文中可以被理解为 对集群的读写操作，“提交”在上下文中可以理解为操作成功。**

首先，Raft集群必须存在一个主节点（leader），我们作为客户端向集群发起的所有操作都必须经由主节点处理。所有Raft算法的核心一部分就是**选主（leader election）**。

主节点会负责接收客户端发过来的操作请求，将操作包装成日志同步给其他节点，在保证大部分节点都同步了本次操作之后，就可以安全地给客户端回应响应了。这部分工作在Raft核心算法中被称之为**日志复制（log replication）**。

在节点们选主的时候，只有符合条件的节点才可以当选主节点。并且为了保证集群对外展现的一致性，不可以覆盖或删除 前任节点已经处理成功的操作日志。其实就是在选主和提交日志的时候进行一些限制，这部分在Raft核心算法中被称之为**安全性（Safety）**。

由**选主（leader election）、日志复制（log replication）、安全性（safety）**。这三部分共同实现了Raft核心的**共识和容错机制**。

- 第一个是关于日志无限增长的问题。

Raft 将操作包装成为了日志，集群每个节点都维护了一个不断增长的日志序列，状态机只有通过重放日志序列来得到。但由于这个日志序列可能会随着时间流逝不断增长，因此我们必须有一些办法来避免无休止的磁盘占用和过久的日志重放。这一部分叫**日志压缩**（**Log compaction**）。

- 第二个是关于集群成员变更的问题。

一个 Raft 集群不太可能永远是固定几个节点，总有扩缩容的需求，或是节点宕机需要替换的时候。直接更换集群成员可能会导致严重的**脑裂**问题。Raft 给出了一种安全变更集群成员的方式。这一部分叫**集群成员变更**（**Cluster membership change**）。

## 选主

### 简介

选主（leader election）就是在分布式系统内抉择出一个主节点来负责一些特定的工作。在执行了选主过程后，集群中每个节点都会识别出一个特定的、唯一的节点作为leader。

### Raft选主过程

#### 节点角色

- **leader**：所有请求的处理者，接收客户端发起的操作请求，写入本地日志后同步至集群其它节点。
- **follower**：请求的被动更新者，从 leader 接收更新请求，写入本地文件。如果客户端的操作请求发送给了 follower，会首先由 follower 重定向给 leader。
- **Candidate**：如果 follower 在一定时间内没有收到 leader 的心跳，则判断 leader 可能已经故障，此时启动 leader election 过程，本节点切换为 candidate 直到选主结束。

#### 任期

没开始一次新的选举，称为一个**任期（term）**，每个term都有一个严格递增的证书与之关联。

每当candidate触发leader election时都会增加term，如果一个candidate赢得选举，他将在本term中担任leader的角色。但斌不是每个term都一定对应一个leader，有时候某个term内会由于选举超时导致选不出leader，这是candidate会递增term号并开始一轮新的选举。

```text
Term 更像是一个逻辑时钟（logic clock）的作用，有了它，就可以发现哪些节点的状态已经过期。每一个节点都保存一个 current term，在通信时带上这个 term 号。
节点间通过 RPC 来通信，主要有两类 RPC 请求：

RequestVote RPCs: 用于 candidate 拉票选举。
AppendEntries RPCs: 用于 leader 向其它节点复制日志以及同步心跳。
```

### 节点状态切换

