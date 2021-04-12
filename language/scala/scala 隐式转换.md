# scala 隐式转换

## <font color="green">摘要</font>

> Scala的隐式转换定义了一套良好的查找机制，当代码出现类型编译错误的时候，编译器试图去寻找一个隐式`implicit`的转换函数，转换出正确的类型，从而使得编译器能够自我修复，完成编译。
> 

## <font color="green">隐式转换类型</font>

1. 隐式参数
2. 隐式视图
3. 隐式类

### <font color="green">隐式参数</font>

> 隐式参数是当编译器找不到函数所需要的某种类型参数时的一种修复机制。
> 

```scala
implicit implicit_val = ""
```

### <font color="green">隐式视图</font>

**隐式视图包含两种转换类型：隐式类型转换以及隐式方法调用。**

> 隐式视图，是指把一种类型转化为另一种类型，以符合表达式的要求。
> 

```scala
implicit def <ConversionName>(<argumentName>: OriginalType): ViewType
```

上述定义形式。在需要的收，如果隐式作用域里存在这个定义，它会隐式地把`OriginalType`类型转换为`ViewType`类型的值。

> 隐式类型转换是编译器发现传递的数据类型与申明的不一致时，编译器在当前作用域查抄类型转换方法，对数据类型进行转换。
> 

### <font color="green">隐式类</font>

> scala**2.10**引入。隐式类代指的是使用implicit关键字修饰的类。
> 在作用域内，标志有implicit关键字的类的著构造器可用于隐式转换。
> 

**NOTE:**

- 构造器参数有且只有一个，且为非隐式参数。
- 隐式类必须被定义在类、伴生对象以及包对象中。
- 隐式类不可以是`case class`。（`case class`会自动生成伴生对象）
- 同一作用域内不可有与之同名的标识符。
- 注意二义性。