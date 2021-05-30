# ThreadLocal内存泄漏

## Preface

Thread Local的使用是为了提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。

![img](E:\git_files\hwloser\study-notes\language\java\picture\v2-45affd67cf3dfb5637878d8f46ea5061_720w.jpg)

### description

每个Thread维护一个ThreadLocalMap映射表，这个映射表的key时ThreadLocal实例本身，value时真正需要存储的Object。

也就是说ThreadLocal本身并不直接存储value。它只是作为一个key来让线程从ThreadLocalMap获取value。

**note: 虚线表示的是weak reference，会在下一次gc的时候被回收掉。**

所以在 key 被回收之后，ThreadLocalMap中的entry还留存了key为null的ThreadLocal对象。