# java future 

# Future interface

定义：

![](B:\study-notes\java\illustration\future_structure.png)

接口定义行为，我们通过上图可以看到实现Future接口的子类会具有哪些行为：

- 我们可以取消这个执行逻辑，如果这个逻辑已经正在执行，提供可选的参数来控制是否取消已经正在执行的逻辑。
- 我们可以判断执行逻辑是否已经被取消。
- 我们可以判断执行逻辑是否已经执行完成。
- 我们可以获取执行逻辑的执行结果。
- 我们可以允许在一定时间内去等待获取执行结果，如果超过这个时间，抛`TimeoutException`。

## FutureTask

FutureTask是Future的具体实现。`FutureTask`实现了`RunnableFuture`接口。`RunnableFuture`接口又同时继承了`Runnable` 和 `Runnable` 接口。所以`FutureTask`既可以作为Runnable被线程执行，又可以作为Future得到Callable的返回值。