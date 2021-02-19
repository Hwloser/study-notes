# java util function

## summary

![function导图](B:\study-notes\java\illustration\functional_interface.png)

1. Function：接收参数，并返回结果。

```java
R apply(T t);
```

2. Consumer：接收参数，无返回结果。

```java
void accept(T t);
```

3. Supplier：不接收参数，但返回结果。

```java
T get();
```

4. Predicate：接收参数，返回boolean值。

```java
boolean test(T t);
```

## lambda 结构

- **一个Lambda表达式可以有0个或多个参数，参数的类型可以明确声明，也可以通过上下文来推断。例如（int a）和（a）效果一样；**
- **所有参数都必须包含在圆括号内，参数之间用逗号相隔；**
- **空圆括号代表参数集为空。例如：（）-> 42**
- **当只有一个参数，且其类型可以推导出时，圆括号（）可以省略。例如：a -> return a\*a**
- **Lambda表达式的主体也就是body可以包含0条或多条语句。**
- **如果表达式的主体只有一条语句，花括号{}可以省略，匿名函数的返回类型与该主体表达式一致**
- **如果表达式的主体包含一条语句以上，则必须包含在花括号{}里面形成代码块。匿名函数的返回类型与该主体表达式一致，若没有返回则为空。**
- **statement和expression的区别，expression只有一句，不需要花括号包裹，不需要return；statement需要花括号包裹，且如果有返回值，必须return**