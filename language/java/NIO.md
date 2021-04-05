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

### <font color="green">容量位置和限制</font>

#### 容量（capacity）

作为一块内存，buffer有一个固定的大小，叫做capacity（容量）。也就是说，最多只能写入 capacity大小的数据。一旦buffer写满了就需要清空一度的数据一边下次继续写入新的数据。

#### 位置（position）

当写入数据到buffer的时候需要从一个确定的位置开始，默认初始化这个position为0。一旦写入了数据，那么position的值就会只想数据之后的单元，position最大可以指定到 `capacity - 1`.

#### 上限（limit）

在**写模式**，limit的含义是所能写入的最大容量，它等同于buffer的capacity。

一旦切换到**读模式**，limit则能代表所能读取的最大数据量，它的值等同于写模式下的position的位置。