# type keyword

## 声明类型

scala里的类型，除了在定义`class`,`trait`,`object`时会产生类型，还可以通过`type`关键字来声明类型。

type相当于声明一个类型别名。

e.g. 

```scala
type People = List[Person]
```

**代码编译之后没有什么神奇的，仅仅是把所有出现People这个字眼的地方都用List of Person替代了。**

```java
public List<Person> teenagers(List<Person> people)
{
  return (List)people.filter(new AbstractFunction1() { public static final long serialVersionUID = 0L;

    public final boolean apply(Person person) { return person.age() < 20; }
  });
}
```

## 给一类操作命名

```scala
type PersonPredicate = Person => Boolean
```

**所有这种接受一个参数，返回一个值的操作都是Function1<Person, Object>.**

### TIPS:

如果我们真的定义一个超过二十二个参数的操作会如何呢？

**type Function23 is not a member of package scala**