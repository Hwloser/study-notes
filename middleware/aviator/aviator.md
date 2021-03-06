# Avaitor

> Aviator是一个高性能、轻量级的java语言实现的表达式求值引擎，主要用于各种表达式的动态求值。现在已经有很多开源可用的java表达式求值引擎，为什么还需要Avaitor呢？
> Aviator的设计目标是轻量级和高性能 ，相比于Groovy、JRuby的笨重，Aviator非常小，加上依赖包也才450K,不算依赖包的话只有70K；当然，Aviator的语法是受限的，它不是一门完整的语言，而只是语言的一小部分集合。
> 其次，Aviator的实现思路与其他轻量级的求值器很不相同，其他求值器一般都是通过解释的方式运行，而Aviator则是直接将表达式编译成Java字节码，交给JVM去执行。简单来说，Aviator的定位是介于Groovy这样的重量级脚本语言和IKExpression这样的轻量级表达式引擎之间。

## <font color="green">特性</font>

### <font color="green">优势</font>

- 支持大部分运算操作符
  - 算数运算符
  - 关系运算符
  - 逻辑运算符
  - 正则匹配操作符（=~）
  - 三元表达式（?:）
  - **并且支持操作符的优先级和括号强制优先级**
- 支持函数调用和自定义函数
- 支持正则表达式匹配，类似Ruby、Perl的匹配语法，并且支持类Ruby的`$digit`指向匹配分组
- 自动类型转换，当执行操作的时候，会自动判断操作类型并作出相应的转换，无法转换时即抛出异常
- 支持传入变量，支持类似`a.b.c`的嵌套变量的访问
- 性能卓越

### <font color="green">劣势（限制）</font>

- 没有`if`，`else`，`do while`等语句，没有复制语句，仅支持逻辑表达式、算术表达式、三元表达式和正则匹配
- 没有位运算

## <font color="green">依赖jars packages</font>

**commons-beanutils和commons-logging。**




