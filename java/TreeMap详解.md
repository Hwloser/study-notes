# TreeMap详解

## summary

TreeMap是基于红黑树的实现，也是记录了key-value的映射关系，该映射根据key的自然排序进行排序或者根据构造方法中传入的compartor进行排序。

1. TreeMap是红黑树的implementation。
2. TreeMap是有序的key-value集合。

## TreeMap jdk实现

![img](TreeMap%E8%AF%A6%E8%A7%A3.assets/4761309-4b6860d66b903c54.png)

1. **Map interface：**定义将key映射value的行为 ，Map规定不能包含重复的key值，每个key最多且只能有一个值。**mep用于替换Dictionary abstract class**。
2. **AbstractMap 类：** 提供了一个Map骨架的实现,尽量减少了实现Map接口所需要的工作量。
3. **SortedMap 接口：** 定义按照key排序的Map结构，规定key-value是根据键key值的自然排序进行排序的，或者根据构造key-value时设定的构造器进行排序。
4. **NavigableMap 接口：** 是SortedMap接口的子接口，在其基础上扩展了针对搜索目标返回最近匹配项的导航方法，例如方法lowEntry、floorEntry、ceilingEntry等，如果不存在这样的键，则返回null。
5. **Cloneable 接口：** 实现了该接口的类可以显示的调用Object.clone()方法，合法的对该类实例进行字段复制，如果没有实现Cloneable接口的实例上调用Obejct.clone()方法，会抛出CloneNotSupportException异常。正常情况下，实现了Cloneable接口的类会以公共方法重写Object.clone()。
6. **Serializable 接口：** 实现了该接口标示了类可以被序列化和反序列化。



