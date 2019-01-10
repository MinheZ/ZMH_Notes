<!-- GFM-TOC -->
* [泛型](#泛型)
    * [1 简单泛型](#简单泛型)
    * [2 泛型接口](#泛型接口)
    * [3 泛型方法](#泛型方法)


-----------------------------

# 泛型
许多原因促进了泛型的出现，最引人入目的原因就是为了创建容器类

## 简单泛型
首先定义一个简单的Holder类：
```java
public class Holder<T>{
    private T a;
    public Holder(T a){
        this.a = a;
    }
    public void set(T a){this.a = a;}
    public T get(){return a;}
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
