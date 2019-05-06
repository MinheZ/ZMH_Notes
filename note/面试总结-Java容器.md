* [1 Java容器有哪些](#1-Java容器有哪些)
* [2 Collection 和 Collections 有什么区别](#2-Collection-和-Collections-有什么区别)
* [3 HashMap在JDK1.7和在1.8有哪些区别](#3-HashMap在JDK1.7和在1.8有哪些区别)
* [4 怎么让 HashMap 变得线程安全](#4-怎么让-HashMap-变得线程安全)

--------------

## 1 Java容器有哪些
- Collection
    - List
        - ArrayList
        - LinkedList
        - Vector(已过时)
    - Set
        - HashSet
        - LinkedHashSet
        - TreeSet
    - Map
        - HashMap
            - LinkedHashMap
        - TreeMap
        - ConcurrentHashMap
        - Hashtable(已过时)

## 2 Collection 和 Collections 有什么区别

**java.util.Collection** 是一个集合接口。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式。

**java.util.Collections** 是一个包装类。它包含有各种有关集合操作的静态多态方法。此类不能实例化，就像一个工具类，服务于Java的Collection框架。

## [3 HashMap在JDK1.7和在1.8有哪些区别？](https://blog.csdn.net/qq_36520235/article/details/82417949)

利用`LinkedHashMap`实现自定义策略的LRU缓存。比如我们可以根据节点数量判断是否移除最近最少被访问的节点，或者根据节点的存活时间判断是否移除该节点等。本节所实现的缓存是基于判断节点数量是否超限的策略。在构造缓存对象时，传入最大节点数。当插入的节点数超过最大节点数时，移除最近最少被访问的节点。实现代码如下：
```java
public class SimpleCache<K, V> extends LinkedHashMap<K, V> {

    private static final int MAX_NODE_NUM = 100;

    private int limit;

    public SimpleCache() {
        this(MAX_NODE_NUM);
    }

    public SimpleCache(int limit) {
        super(limit, 0.75f, true);
        this.limit = limit;
    }

    public V save(K key, V val) {
        return put(key, val);
    }

    public V getOne(K key) {
        return get(key);
    }

    public boolean exists(K key) {
        return containsKey(key);
    }

    /**
     * 判断节点数是否超限
     * @param eldest
     * @return 超限返回 true，否则返回 false
     */
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > limit;
    }
}
```

## 4 怎么让 HashMap 变得线程安全

[如何让HashMap变成线程安全的？](https://blog.csdn.net/u010653908/article/details/53419685)

