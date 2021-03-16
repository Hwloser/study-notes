# Why Use Protocol Buffers

> How do you serialize and retrieve structured data like this? There are a few ways to solve this problem:

- 使用Java序列化。这是默认的方法，因为它已内置在语言中，但是它存在许多众所周知的问题（请参见Josh Bloch的213页的有效Java），如果您需要与之共享数据，也无法很好地工作。用C ++或Python编写的应用程序。
- 您可以发明一种将数据项编码为单个字符串的特殊方法，例如将4个整数编码为“ 12：3：-23：67”。尽管确实需要编写一次性编码和解析代码，但是这是一种简单且灵活的方法，并且解析会带来少量的运行时成本。这对于编码非常简单的数据最有效。
- 将数据序列化为XML。由于XML是人类（一种）可读的，并且存在用于多种语言的绑定库，因此该方法可能非常有吸引力。如果要与其他应用程序/项目共享数据，这可能是一个不错的选择。但是，众所周知，XML占用大量空间，对它进行编码/解码会给应用程序带来巨大的性能损失。同样，导航XML DOM树比通常导航类中的简单字段要复杂得多。

## define proto files

```protobuf
syntax = "proto2";

package tutorial;

option java_package = "com.example.tutorial";
option java_outer_classname = "AddressBookProtos";

message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    optional string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook {
  repeated Person people = 1;
}
```

.proto文件以程序包声明开头，这有助于防止不同项目之间的命名冲突。 在Java中，除非您已明确指定java_package，否则将包名称用作Java包。即使您确实提供了java_package，也仍然应该定义一个普通的包，以避免协议缓冲区名称空间以及非Java语言中的名称冲突。

程序包声明后，您可以看到两个特定于Java的选项：java_package和java_outer_classname。 java_package指定您所生成的类应使用哪个Java包名称。如果未明确指定，则它仅与程序包声明中给出的程序包名称匹配，但是这些名称通常不是适当的Java程序包名称（因为它们通常不以域名开头）。 java_outer_classname选项定义了类名称，该名称应包含此文件中的所有类。如果您没有明确给出java_outer_classname，它将通过将文件名转换为大写驼峰来生成。例如，默认情况下，“ my_proto.proto”将使用“ MyProto”作为外部类名称。

接下来，您将拥有消息定义。消息只是包含一组类型字段的汇总。许多标准的简单数据类型可用作字段类型，包括bool，int32，float，double和string。您还可以通过使用其他消息类型作为字段类型来为消息添加进一步的结构-在上面的示例中，“个人”消息包含PhoneNumber消息，而“地址簿”消息包含“个人”消息。您甚至可以定义嵌套在其他消息中的消息类型-如您所见，PhoneNumber类型是在Person内部定义的。如果您希望某个字段具有一个预定义的值列表之一，也可以定义枚举类型-在这里您要指定电话号码可以是MOBILE，HOME或WORK之一。

每个元素上的“ = 1”，“ = 2”标记标识该字段在二进制编码中使用的唯一“标记”。标签编号1至15与较高的编号相比，编码所需的字节减少了一个字节，因此，为了进行优化，您可以决定将这些标签用于常用或重复的元素，而将标签16和更高的标签用于较少使用的可选元素。重复字段中的每个元素都需要重新编码标签号，因此重复字段是此优化的最佳候选者。

### Each field must be annotated with one of the following modifiers:

- `optional`：可能会或可能不会设置该字段。如果未设置可选字段值，则使用默认值。对于简单类型，您可以指定自己的默认值，就像在示例中为电话号码类型所做的那样。否则，将使用系统默认值：数字类型为零，字符串为空字符串，布尔值为false。对于嵌入式消息，默认值始终是消息的“默认实例”或“原型”，没有设置任何字段。调用访问器以获取未显式设置的可选（或必填）字段的值始终会返回该字段的默认值。
-  `repeated`：该字段可以重复任意次（包括零次）。重复值的顺序将保留在协议缓冲区中。将重复字段视为动态大小的数组。
-  `required`：必须提供该字段的值，否则该消息将被视为“未初始化”。尝试构建未初始化的消息将引发RuntimeException。解析未初始化的消息将引发IOException。除此之外，必填字段的行为与可选字段完全相同。

