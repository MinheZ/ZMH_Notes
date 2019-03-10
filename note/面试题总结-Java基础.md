* [Java基础](#Java基础)

    * [1 JDK和JRE的区别](#JDK和JRE的区别)
    * [2 equals和==的区别](#equals和==的区别)
    * [3 equals与hashcode间的关系](#equals与hashcode间的关系)
    * [4 String, StringBuilder和StringBuffer区别以及线程安全](#String,-StringBuilder和StringBuffer区别以及线程安全)
    * [5 final 的作用](#final-的作用)
    * [6 Math.round](#Math.round)

    

----------------------------

# Java基础

### JDK和JRE的区别

**JRE： Java Runtime Environment**

是 Java 运行时环境，包含了 Java 虚拟机， Java 基础类库。是使用 Java 语言编写的程序运行所需要的软件环境。

**JDK：Java Development Kit** 

Java 开发工具包，是程序员使用 Java 语言编写 Java程序所需要的开发工具包。JDK 包含了 JRE，同时还有包含了编译 Java 源码的编译器 javac，还包含了 Java 程序编写所需要的文档和 demo 例子程序。

----------------------

### equals和==的区别

- 基本数据类型：boolean, byte, char, short, int, long, float, double。它们之间的比较用 `==`。
- 引用数据类型：当它们用`==`的时候，比较的是在内存中的存放地址。

#### equals()方法
Java Object 类中定义了一个 equals 方法
```java
public boolean equals(Object obj) {
    //this - s1
    //obj - s2
    return (this == obj);
}
```
这个方法的初始默认行为是比较对象的内存地址值，一般来说，意义不大。所以，在一些类库当中这个方法被重写了，如String、Integer、Date。在这些类当中equals有其自身的实现（一般都是用来比较对象的成员变量值是否相同），而不再是比较类在堆内存中的存放地址了。

所以说，对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是内存中的存放位置的地址值，跟双等号（==）的结果相同；如果被复写，按照复写的要求来。

--------------------

### equals与hashcode间的关系
hashCode()：计算出对象实例的哈希码，并返回哈希码，又称为散列函数。根类Object的hashCode()方法的计算依赖于对象实例的D（内存地址），故每个Object对象的hashCode都是唯一的；当然，当对象所对应的类重写了hashCode()方法时，结果就截然不同了。

- 两个obj，如果equals()相等，hashCode()一定相等。
- 两个obj，如果hashCode()相等，equals()不一定相等（Hash散列值有冲突的情况，虽然概率很低）。

所以在集合中，判断2个对象是否相等，一般都是先判断`hashCode`是否相等，相等则判断`equals()`，否则返回 false 。

#### 如果重写equals没有重写hashcode会发生什么?
 在存储散列集合时(如Set类)，如果 原对象.equals(新对象)，但没有对hashCode重写，即两个对象拥有不同的hashcode，则在集合中将会存储两个值相同的对象，从而导致混淆。因此在重写equals方法时，必须重写hashcode方法。

--------------------

### String, StringBuilder和StringBuffer区别以及线程安全
- **String:** 字符串常量，每次对 String 类型进行改变的时候就等同于生成一个新的 String 对象，并将指针指向新的 String 对象。所以经常改变内容的的字符串最好不要用 String，因为每次生成对象都会对系统的性能产生影响，当内存中无引用对象增多之后， JVM 的 GC 就开始工作。
- **StringBuffer:** 字符串变量，线程安全，所有的方法都有`synchronized`同步。
- **StringBuilder:** 字符串变量，非线程安全，主要方法与`StringBuffer`相同，

通常情况下，`StringBuffer` 字符串拼接速度大于`String`，除下面特例：
```java
String str = “Hello” + “ ” + “world”;
StringBuffer builder = new StringBuilder("Hello").append(" ").append("world");
```
JVM 直接把 String 翻译成`str = "Hello world";`，因此速度很快。但是对不同字符串的拼接， String 类型的就很慢了。

**对于三者使用的总结：**

    1.如果要操作少量的数据用 = String
    2.单线程操作字符串缓冲区 下操作大量数据 = StringBuilder
    3.多线程操作字符串缓冲区 下操作大量数据 = StringBuffer

-------------------

### final 的作用

final 可修饰数据、方法和类。

- **final 数据：** 一个永远不改变的编译时常量。
- **final 方法：** 方法锁定，防止任何继承类修改它的含义。类中所有 private 方法都隐式地指定为 final。
- **final 类：** 不允许继承该类。

-----------------------------------

### Math.round

口诀：+0.5后向下取整。	`Math.round(-2.5) = -2`

`Math.ceil(double a)`向上取整。

`Math.floor(double a)`向下取整。

