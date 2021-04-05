# LockSupport、Unsafe

## summary

在Java多线程中，当需要阻塞或者唤醒一个线程时，都会使用LockSupport工具类来完成相应的工作。LockSupport定义了一组公共静态方法，这些方法提供了最基本的线程阻塞和唤醒功能，而LockSupport也因此成为了构建同步组件的基础工具。

LockSupport定义了一组以park开头的方法用来阻塞当前线程，以及unpark(Thread)方法来唤醒一个被阻塞的线程，这些方法描述如下：

| 方法名称                      | 描 述                                                        |
| ----------------------------- | ------------------------------------------------------------ |
| void park()                   | 阻塞当前线程，如果掉用unpark(Thread)方法或被中断，才能从park()返回 |
| void parkNanos(long nanos)    | 阻塞当前线程，超时返回，阻塞时间最长不超过nanos纳秒          |
| void parkUntil(long deadline) | 阻塞当前线程，直到deadline时间点                             |
| void unpark(Thread)           | 唤醒处于阻塞状态的线程                                       |

在Java 5之前，当线程使用synchronized关键字阻塞在一个对象上时，通过线程dump能够看到该线程的阻塞对象，而Java 5推出的Lock等并发工具却遗漏了这一点，致使在线程dump时无法提供阻塞对象的信息。因此，在Java 6中，LockSupport新增了上述3个含有阻塞对象的方法，用以替代原有的park方法。

## Unsafe

Unsafe类就和它的名字一样，是一个比较危险的类，它主要用于执行低级别、不安全的方法。尽管这个类和所有的方法都是公开的（public），但是这个类的使用仍然受限，你无法在自己的java程序中直接使用该类，因为只有授信的代码才能获得该类的实例。如果我们要使用Unsafe类，首先需要获取Unsafe类的对象，但是它的构造函数是private的：

```java
private Unsafe() {}
```

```java
@CallerSensitive
public static Unsafe getUnsafe() {
    Class var0 = Reflection.getCallerClass();
    if (!VM.isSystemDomainLoader(var0.getClassLoader())) {
        throw new SecurityException("Unsafe");
    } else {
        return theUnsafe;
    }
}
```

```java
/**
 * Returns the class loader for the class.  Some implementations may use
 * null to represent the bootstrap class loader. This method will return
 * null in such implementations if this class was loaded by the bootstrap
 * class loader.
 *
 * <p> If a security manager is present, and the caller's class loader is
 * not null and the caller's class loader is not the same as or an ancestor of
 * the class loader for the class whose class loader is requested, then
 * this method calls the security manager's {@code checkPermission}
 * method with a {@code RuntimePermission("getClassLoader")}
 * permission to ensure it's ok to access the class loader for the class.
 *
 * <p>If this object
 * represents a primitive type or void, null is returned.
 *
 * @return  the class loader that loaded the class or interface
 *          represented by this object.
 * @throws SecurityException
 *    if a security manager exists and its
 *    {@code checkPermission} method denies
 *    access to the class loader for the class.
 * @see java.lang.ClassLoader
 * @see SecurityManager#checkPermission
 * @see java.lang.RuntimePermission
 */
@CallerSensitive
public ClassLoader getClassLoader() {
    ClassLoader cl = getClassLoader0();
    if (cl == null)
        return null;
    SecurityManager sm = System.getSecurityManager();
    if (sm != null) {
        ClassLoader.checkClassLoaderPermission(cl, Reflection.getCallerClass());
    }
    return cl;
}
```

## Unsafe修改内存 以及 unsafe非Java堆中分配内存

Unsafe类的putInt()和getInt()等方法可以直接修改内存。

```java
public static void unsafePutGetInt() throws Exception {
    class Student {
        private int age = 5;
        
        public int getAge() {
            return age;
        }
    }
    
    Student student = new Student();
    System.out.println(student.getAge());
    
    Field field = student.getClass().getDeclaredField("age");
    unsafe.putInt(student, unsafe.objectFieldOffset(field), 10);
    
    System.out.println(student.getAge());
}
```

使用new关键字分配的内存会在堆中，并且对象的生命周期内，会被垃圾回收器管理。unsafe类通过allocateMemory(long)方法分配的内存，不受integer.MAX_VALUE的限制，并且分配在非堆内存，使用它时，需要分厂谨慎，该部分的内存需要手动回收，否则会产生内存泄漏；非法的地址访问时，会导致java虚拟机奔溃。在需要分配大的连续区域、实时编程时，可以使用该方式，java的nio使用了这一方法。

```java
public static void unsafeAllocateMemory() throws Exception {
  int BYTE = 1;
  long address = unsafe.allocateMemory(BYTE);
  
  unsafe.putByte(address, (byte) 10);
  unsafe.getByte(address);
  
  unsafe.freeMemory(address);
}
```

## 提供CAS原子操作

Unsafe类中提供了compareAndSwapObject()、compareAndSwapInt()、和 compareAndSwapLong()这三个方法用来实现对应的CAS原子操作。在java的并发编程中用到的CAS操作。在Java的并发编程中用到的CAS操作都是调用的Unsafe类的相关方法。

```java
public class TestUnsafe {
    private volatile int value = 0;
    private long valueOffset;

    private Unsafe unsafe;

    public TestUnsafe(Unsafe unsafe) throws NoSuchFieldException {
        this.unsafe = unsafe;

        Field valueField = TestUnsafe.class.getDeclaredField("value");
        valueOffset = unsafe.objectFieldOffset(valueField);
    }

    public void increment() {
        int oldValue = this.value;
        for (int i = 0; i < 10; i++) {
            if (unsafe.compareAndSwapInt(this, valueOffset, oldValue, oldValue + 1)) {
                break;
            }
            oldValue = value;
        }
    }
}

```



