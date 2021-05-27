# Caffeine

![image-20201121143735194](picture/1460000038665527)

## architecture

- 存储介质：Cache内部包含着一个ConcurrentHashMap，作为数据的存储容器。
- Scheduler：定期清空数据的一个机制，可以不设置，如果不设置则不会主动清空过期数据。
- Executor：指定运行异步任务时要使用的线程池。可以不设置，如果不设置则会使用默认的线程池，也就是`ForkJoinPool.commonPool()`。

## 优美的数据填充机制

### <font color="green">put数据</font>

- 手动加载
- 同步加载
- 异步加载

### <font color="green">数据淘汰机制</font>

- 基于权重
- 基于大小
- 基于时间
- 基于引用

#### <font color="green">基于引用的淘汰机制</font>

使用异步加载的方式不允许使用`reference policy`淘汰机制。

使用引用太太机制的时候判断两个`key`或者`value`是否相同，使用的是`==`，而非是`equals()`，也就是说需要两个`key`指向同一个对象才能被认为是一致的，这样极可能导致缓存命中出现预料之外的问题。
