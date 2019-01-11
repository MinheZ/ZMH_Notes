<!-- GFM-TOC -->
* [泛型](#泛型)
    * [1 简单泛型](#简单泛型)
    * [2 泛型接口](#泛型接口)
    * [3 泛型方法](#泛型方法)
    * [4 通配符](#通配符)


-----------------------------

# 泛型
许多原因促进了泛型的出现，最引人入目的原因就是为了创建容器类。

**声明：** 本章[转载](http://www.importnew.com/24029.html)自他处，侵权即删。

## 简单泛型
首先定义一个简单的Box类：
```java
public class Box<T> {
    // T stands for "Type"
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```
Java泛型的核心概念：告诉编译器想使用什么类型，然后编译器帮你实现所有的细节。

## 泛型接口
泛型也可以用于接口。例如生成器(generator)，这是一种专门负责创建对象的类。一般而言，一个生成器只定义一种方法。
```java
public interface Generator<T>{
    T next();
}
```
Java泛型的一个局限性：基本类型无法作为参数类型。但是在Java SE5具备了自动装箱和拆箱的功能了。

## 泛型方法
要定义泛型方法，只需要将泛型参数列表置于返回值之前：
```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}
public class Pair<K, V> {
    private K key;
    private V value;
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```
我们可以像下面这样去调用泛型方法：
```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
```
或者在Java1.7/1.8利用type inference，让Java自动推导出相应的类型参数：
```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
```

## 边界符
查找一个泛型数组中大于某个特定元素的个数，我们可以这样实现：
```java
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}
```
但是这样很明显是错误的，因为除了short, int, double, long, float, byte, char等原始类型，其他的类并不一定能使用操作符>，所以编译器报错，那怎么解决这个问题呢？答案是使用边界符。
```java
ublic interface Comparable<T> {
    public int compareTo(T o);
}
```
做一个类似于下面这样的声明，这样就等于告诉编译器类型参数T代表的都是实现了Comparable接口的类，这样等于告诉编译器它们都至少实现了compareTo方法。
```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}
```

## 通配符
在了解通配符之前，我们首先必须要澄清一个概念，还是借用我们上面定义的Box类，假设我们添加一个这样的方法：
```java
public void boxTest(Box<Number> n){}
```
Box<Number>中能接受什么样的参数类型呢？我们是否可以传入Box<Integer>呢？答案是否定的，虽然Integer是Number的子类，但是在泛型中Box<Number>和Box<Integer>之间并没有任何联系。举个例子，首先顶底几个简单的类：
```java
class Fruit {}
class Apple extends Fruit {}
class Orange extends Fruit {}
```
下面这个例子中，我们创建了一个泛型类Reader，然后在f1()中当我们尝试Fruit f = fruitReader.readExact(apples);编译器会报错，因为List<Fruit>与List<Apple>之间并没有任何的关系。
```java
public class GenericReading {
    static List<Apple> apples = Arrays.asList(new Apple());
    static List<Fruit> fruit = Arrays.asList(new Fruit());
    static class Reader<T> {
        T readExact(List<T> list) {
            return list.get(0);
        }
    }
    static void f1() {
        Reader<Fruit> fruitReader = new Reader<Fruit>();
        // Errors: List<Fruit> cannot be applied to List<Apple>.
        // Fruit f = fruitReader.readExact(apples);
    }
    public static void main(String[] args) {
        f1();
    }
}
```
但是按照我们通常的思维习惯，Apple和Fruit之间肯定是存在联系，然而编译器却无法识别，那怎么在泛型代码中解决这个问题呢？我们可以通过使用通配符来解决这个问题：
```java
static class CovariantReader<T> {
    T readCovariant(List<? extends T> list) {
        return list.get(0);
    }
}
static void f2() {
    CovariantReader<Fruit> fruitReader = new CovariantReader<Fruit>();
    Fruit f = fruitReader.readCovariant(fruit);
    Fruit a = fruitReader.readCovariant(apples);
}
public static void main(String[] args) {
    f2();
}
```
这样就相当与告诉编译器， fruitReader的readCovariant方法接受的参数只要是满足Fruit的子类就行(包括Fruit自身)，这样子类和父类之间的关系也就关联上了。

## PECS 原则
上面我们看到了类似<? extends T>的用法，利用它我们可以从list里面get元素，那么我们可不可以往list里面add元素呢？：
```java
public class GenericsAndCovariance {
    public static void main(String[] args) {
        // Wildcards allow covariance:
        List<? extends Fruit> flist = new ArrayList<Apple>();
        // Compile Error: can't add any type of object:
        // flist.add(new Apple())
        // flist.add(new Orange())
        // flist.add(new Fruit())
        // flist.add(new Object())
        flist.add(null); // Legal but uninteresting
        // We Know that it returns at least Fruit:
        Fruit f = flist.get(0);
    }
}
```
答案是否定，Java编译器不允许我们这样做，为什么呢？对于这个问题我们不妨从编译器的角度去考虑。因为List<? extends Fruit> flist它自身可以有多种含义：
```java
List<? extends Fruit> flist = new ArrayList<Fruit>();
List<? extends Fruit> flist = new ArrayList<Apple>();
List<? extends Fruit> flist = new ArrayList<Orange>();
```
当我们尝试add一个Fruit的时候，这个Fruit可以是任何类型的Fruit，而flist可能只想某种特定类型的Fruit，编译器无法识别所以会报错。

所以对于实现了<? extends T>的集合类只能将它视为Producer向外提供(get)元素，而不能作为Consumer来对外获取(add)元素。

如果我们要add元素应该怎么做呢？可以使用<? super T>：
```java
public class GenericWriting {
    static List<Apple> apples = new ArrayList<Apple>();
    static List<Fruit> fruit = new ArrayList<Fruit>();
    static <T> void writeExact(List<T> list, T item) {
        list.add(item);
    }
    static void f1() {
        writeExact(apples, new Apple());
        writeExact(fruit, new Apple());
    }
    static <T> void writeWithWildcard(List<? super T> list, T item) {
        list.add(item)
    }
    static void f2() {
        writeWithWildcard(apples, new Apple());
        writeWithWildcard(fruit, new Apple());
    }
    public static void main(String[] args) {
        f1(); f2();
    }
}
```
这样我们可以往容器里面添加元素了，但是使用super的坏处是以后不能get容器里面的元素了，原因很简单，我们继续从编译器的角度考虑这个问题，对于List<? super Apple> list，它可以有下面几种含义：
```java
List<? super Apple> list = new ArrayList<Apple>();
List<? super Apple> list = new ArrayList<Fruit>();
List<? super Apple> list = new ArrayList<Object>();
```
当我们尝试通过list来get一个Apple的时候，可能会get得到一个Fruit，这个Fruit可以是Orange等其他类型的Fruit。

根据上面的例子，我们可以总结出一条规律，“Producer Extends, Consumer Super”：
- “Producer Extends” – 如果你需要一个只读List，用它来produce T，那么使用? extends T。
- “Consumer Super” – 如果你需要一个只写List，用它来consume T，那么使用? super T。
- 如果需要同时读取以及写入，那么我们就不能使用通配符了。

如何阅读过一些Java集合类的源码，可以发现通常我们会将两者结合起来一起用，比如像下面这样：
```java
public class Collections {
    public static <T> void copy(List<? super T> dest, List<? extends T> src) {
        for (int i=0; i<src.size(); i++)
            dest.set(i, src.get(i));
    }
}
```
