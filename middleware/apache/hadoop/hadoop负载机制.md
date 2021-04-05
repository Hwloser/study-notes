# hadoop负载机制

## 心跳机制

- 集群节点之间必须要做时间同步。

DateNode默认会每隔3秒向namenode发送一次心跳报告，目的就是告诉namenode，datanode还活着。

相关参数：

```xml
<property>
  <name>dfs.heartbeat.interval</name>
  <value>3</value>
  <description>Determines datanode heartbeat interval in seconds.</description>
</property>
```

namenode会在10次机会中来判定datanode是否活着，如果在10次机会中，也就是10*3 seconds中datanode依旧未向namenode报告自己还活着，namenode就会暂时的认为datanode "死了"。

同样的，重检查间隔时间也是可以通过参数来调整。

```xml
<property>
  <name>dfs.namenode.heartbeat.recheck-interval</name>
  <value>300000</value>
  <description>
    This time decides the interval to check for expired datanodes.
    With this value and dfs.heartbeat.interval, the interval of
    deciding the datanode is stale or not is also calculated.
    The unit of this configuration is millisecond.
  </description>
</property>
```

## 安全模式

namenode在启动的时候会存储一些元数据。元数据一般分为三部分。存储目录树（abstract）；存储数据和块的映射关系；数据块存储的位置信息。

- 存储在内存中

内存中的元数据，读写快，缺点是服务器宕机，内存中的全部数据将会丢失。

- 存储在磁盘中

存储在磁盘中的元数据，可以确保数据持久化。但是每次进行读写操作就会进行磁盘IO，而磁盘的随机读写性能相较于内存读写性能较低。

```txt
hdfs dfsadmin -safemode enter # 进入安全模式
hdfs dfsadmin -safemode leave # 离开安全模式
hdfs dfsadmin -safemode get # 获取当前是否处于安全模式，如果处于则返回on，否则返回off
hdfs dfsadmin -safemode wait #等待自行退出安全模式
```

## 机架策略

### 负载均衡

```xml
<property>
  <name>dfs.datanode.balance.bandwidthPerSec</name>
  <value>1048576</value>
  <description>
        Specifies the maximum amount of bandwidth that each datanode
        can utilize for the balancing purpose in term of
        the number of bytes per second.
  </description>
</property>
```

hadoop 还提供了bash用以手动的进行负载均衡。

```reStructuredText
start-balancer.sh -t 10% 
```