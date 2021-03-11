# Block Manager 详解

## preface

BlockManager是管理整个spark运行时的数据读写，当然也包含数据存储本身。BlockManager也是分布式的。

## BlockManager 

![img](Block%20Manager%20%E8%AF%A6%E8%A7%A3.assets/1005794-20170305235055157-1156699855.png)

## BlockManager 运行实例

### 从application submit 审视 blockmanager

- 在 Application 启动的时候会在 spark-env.sh 中注册 BlockManagerMaster 以及 MapOutputTracker，其中：
  - BlockManagerMaster：对整个集群的Block数据进行管理。
  - MapOutputTracker：跟踪所有Mapper的输出。
- BlockManagerMasterEndpoint 本身是一个消息体，会负责通过 rpc 的方式去管理所有节点的BlockManager。
- 每一个启动 ExceutorBackend 都会实例化 BlockManger 并通过远程通信的方式注册给 BlockManageMaster。实际上是 Executor 中的 BlockManager 注册给 driver 上的 BlockManagerMasterEndpoint。
- MemoryStore 是 BlockManager 中专门负责内存数据存储和读写的类，MemoryStore 是以 block 为单位的。
- DiskStore 是 BlockManager 中专门负责磁盘存储和读写的类。
- DiskBlockManager是管理 LogicalBlock 与 Disk 上的PhysicalBlock 之间的映射关联并负责磁盘的文件的创建，读写等。

### 从job运行的角度来观察BlockManager

1. 首先通过 MemoryStore 来存储广播变量。
2. 在 Driver 中通过 BlockManagerInfo 