# ChannelPipleline

Netty的ChannelPipeline接口设计，就采用了责任链的设计模式，底层采用双向链表的数据结构，将链上的各个handler串联起来。

客户端的每一个请求的到来，ChannelPipeLine中所有的handler都有机会处理request。因此，对于inbound request，全部从头节点开始往后传播。

```java
ChannelPipeline p = ch.pipeline();
p.addLast("1", new InboundHandlerA());
p.addLast("2", new InboundHandlerB());
p.addLast("3", new OutboundHandlerC());
p.addLast("4", new OutboundHandlerD());
p.addLast("5", new InboundOutboundHandlerX());
```

如上的例子：

事件inbound的时候：handler的评估（evaluate）顺序为1、2、3、4、5
事件outbound的时候：handler的评估顺序为5、4、3、2、1

### note that

ChannelPipeline虽然初始化了5个handler。但这些handler并不一定全部执行。

- 3、4并未实现ChannelInboundHandler，因此入站事件的实际评估顺序为1、2、5
- 1、2并未实现ChannelOutboundHandler，因此出站事件的实际评估为5、4、3
- 5同时实现了ChannelInboundHandler和ChannelOutboundHandler

