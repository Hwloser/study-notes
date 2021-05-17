# Apache Thrift

## Thrift 分层结构

传输层（Transport Layer）、协议层（Protocol Layer）、处理层（Processor Layer）和服务层（Server Layer）

- 传输层（Transport Layer）：传输层负责直接从网络中读取和写入数据，它定义了具体的网络传输协议。【e.g.：TCP/IP 传输】
- 协议层（Protocol Layer）：协议层定义了数据传输格式，负责网络传输数据的序列化和反序列化。【e.g JSON，XML、二进制数据】
- 处理层（Processor Layer）：处理层有具体的IDL（接口描述语言）生成的，封装了具体的底层网络传输和序列化方式，并委托给用户实现的Handler进行处理。
- 服务层（Server Layer）：整合上述组件，提供具体的 网络线程/IO服务模型。

## Thrift数据基本类型

### 基本类型

- bool: 布尔值
- byte: 8位有符号整数
- i16: 16位有符号整数
- i32: 32位有符号整数
- i64: 64位有符号整数
- double: 64位浮点数
- string: UTF-8编码的字符串
- binary: 二进制串

### 结构体类型

- struct: 定义的结构体对象

### 容器类型

- list：有序元素列表
- set：无需无重复元素集合
- map：有序的key/value集合

### 异常类型

- exception：异常类型

### 服务类型

- service：service类型

## Thrift协议

Thrift可以让用户选择客户端于服务器之间传输通信协议的类型，在传输协议上分为**文本（text）**和**二进制（binary）**传输协议。

- TBinaryProtocol：**二进制**编码格式进行数据传输

- TCompactProtocol：**高效率**的、**密集**的**二进制**编码格式进行数据传输

- TJSONProtocol： 使用`JSON`**文本**的数据编码协议进行数据传输

- TSimpleJSONProtocol：只提供`JSON`**只写**的协议，适用于通过**脚本语言解析**


