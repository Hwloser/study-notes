# NIO

## <font color="green">summarg</font>

**java NIO 核心组件：**

- Channel
- Buffer
- Selector

## <font color="green">Channel和Buffer</font>

通常来说NIO中所有的IO都时从Channel开始的。Channel和Stream有点雷系。通过Channel，我们可以从Channel中巴数据写入到Buffer中，也可以把数据从Buffer写入到Channel中。

**主要使用的一些Channel：**

- FileChannel
- DatagramChannel
- SocketChannel
- ServerSocketChannel

**主要使用的一些Buffer：**

- ByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

## <font color="green">Selector</font>

selector允许单线程操作多个通道。如果你的程序中有大量的链接，同时每个连接的IO单款不高的话，这个特性将会非常有帮助。

**如果要使用Selector的话，必须将Channel注册到Selector上，然后就可以待用Selector中的select()方法。这个方法会进入阻塞，知道有一个channel的状态符合条件。当方法返回之后，线程可以处理这些事件。**

## <font color="green">Java NIO Channel</font>

Java NIO Channel和stream非常的形似。

- channel可以读也可以写，流一般来说是单项的（输入、输出流）
- channel可以异步写
- channel总是基于buffer来读写的

