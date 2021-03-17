# Actor Model

Actor模型是一个概念模型，用于处理并发计算。它定义了一系列系统组件应该如何动作和交互的通用规则。

## <font color="purple">Actors</font>

Actor作为最基本的计算unit。它能接收一个消息并且基于其执行计算。
这个理念很像面向对象语言，一个对象接收一条消息（方法调用），然后根据接收的消息做事（调用了哪个方法）。

> actors之间相互隔离，它们并不互相共享内存。

## <font color="purple">Actors Mailboxes</font>

> 尽管许多actors同时运行，但是一个actor只能顺序地处理消息。

其它actors发送了三条消息给一个actor，这个actor只能一次处理一条。所以如果要并行处理3条消息，则需要把这条消息发给3个actors。
消息异步地传送到actor，所以当actor正在处理消息时，新来的消息应该存储到别的地方。Mailbox就是这些消息存储的地方。

## <font color="purple">Actors Actions</font>

- **Create more actors。**
- **Send messages to other actors。**
- **Designates what to do with the next message。**

## <font color="purple">Fault Tolerance</font>

> Erlang 引入了「随它崩溃」的哲学理念，这部分关键代码被监控着，监控者的唯一职责是知道代码崩溃后干什么（如将这个单元代码重置为正常状态），让这种理念成为可能的正是actor模型。

每段代码都运行在process中（process是erlang称呼actor的方式）。这个process完全独立，意味着它的状态不会影响其他process。我们有个supervisor，实际上它只是另一个process（所有东西都是actor），当被监控的process挂了，supervisor这个process会被通知并对此进行处理。这就让我们能创建「自愈」系统了。如果一个actor到达异常状态并崩溃，无论如何，supervisor都可以根据不同的策略做出相应的反应并尝试把它变成一致状态。

