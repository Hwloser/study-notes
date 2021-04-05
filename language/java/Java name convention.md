# Java name convention

编程过程中，有太多太多让我们头疼的事情了，比如命名、维护其他人的代码、写测试、与其他人沟通交流等等。就连世界级软件大师 Martin Fowler 大神都说过 CS 领域有两大最难的事情，一是缓存失效，一是[程序命名](https://martinfowler.com/bliki/TwoHardThings.html)。

## <font color="green">命名规范为何被重视</font>
**好的命名即是注释，别人一看到你的命名就知道你的变量、方法或者类是做什么的！**

《Clean Code》这本书明确指出：
> 好的代码本身就是注释，我们要尽量规范和美化自己的代码来减少不必要的注释。
> 若编程语言足够有表达力，就不需要注释，尽量通过代码来阐述。

### 例如：

去掉下面复杂的注释，只需要创建一个与注释所言同一事物的函数即可：
```java
// check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```
**应替换为：**

```java
if (employee.isEligibleForFullBenefits())
```

## 常见命名规则以及适用场景

### <font color="green">驼峰命名法（CamelCase）</font>

驼峰命名法应该我们最常见的一个，这种命名方式使用大小写混合的格式来区别各个单词，并且单词之间不使用空格隔开或者连接字符连接的命名方式。

> **类名需要使用大驼峰命名法（UpperCamelCase）。**

正例：
```javav
ServiceDiscovery、ServiceInstance、LruCacheFactory
```
反例：
```java
serviceDiscovery、Serviceinstance、LRUCacheFactory
```

> **方法名、参数名、成员变量、局部变量需要使用小驼峰命名法（lowerCamelCase）。**

正例：
```java
getUserInfo()、createCustomThreadPool()、setNameFormat(String nameFormat)
Uservice userService;
```
反例：
```java
GetUserInfo()、CreateCustomThreadPool()、setNameFormat(String NameFormat)
Uservice user_service
```

### <font color="green">蛇形命名法（snake_case）</font>

> **测试方法名、常量、枚举名称需要使用蛇形命名法（snake_case）。**

在蛇形命名法中，各个单词之间通过下划线“_”连接，比如：
**should_get_200_status_code_when_request_is_valid、CLIENT_CONNECT_SERVER_FAILURE。**

### <font color="green">串式命名法（kebab-case）</font>

在串式命名法中，各个单词之间通过下划线“-”连接，比如dubbo-registry。
建议项目文件夹名称使用串式命名法（kebab-case），比如 dubbo 项目的各个模块的命名是下面这样的。

