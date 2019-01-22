* [HashMap](#HashMap)
    * [1 HashMap的数据结构](#HashMap的数据结构)
    * [2 HashMap源码分析](#HashMap源码分析)

# HashMap
`HashMap`基于哈希表的`Map`接口的实现。此实现提供所有可选的映射操作，并允许使用 `null`值和`null`键。除了不同步和允许使用`null`之外，`HashMap`类与`Hashtable`大致相同。）此类不保证映射的顺序，特别是它不保证该顺序恒久不变。

值得注意的是HashMap不是线程安全的，如果想要线程安全的`HashMap`，可以通过`Collections`类的静态方法`synchronizedMap`获得线程安全的`HashMap`。

## HashMap的数据结构
HashMap的底层主要是基于数组和链表来实现的，它是通过计算散列码来决定存储的位置，HashMap底层是通过[链地址法](https://github.com/MinheZ/Notes/blob/master/note/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.md#%E9%93%BE%E5%9C%B0%E5%9D%80%E6%B3%95)来处理hash冲突的。

首先HashMap里面实现一个静态内部类Entry，其重要的属性有key, value, next，从属性key, value我们就能很明显的看出来Entry就是HashMap键值对实现的一个基础bean，我们上面说到HashMap的基础就是一个线性数组，这个数组就是`Entry[]`，Map里面的内容都保存在`Entry[]`里面。
```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }
    // 实现hashCode
    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }

    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }

    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
```
## HashMap源码分析
### 关键属性
先看HashMap类中的一些关键属性：
```java
transient Node<K,V>[] table;  // 存储元素的表
transient int size;  // map中元素的个数
transient int modCount;  // 被修改的次数
int threshold;  // 阈值，当超过的时候会扩容，threshold = 加载因子*容量
final float loadFactor;  // 装填因子
```
其中，[装填因子](https://github.com/MinheZ/Notes/blob/master/note/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.md#%E4%BA%8C%E6%AC%A1%E6%8E%A2%E6%B5%8B)

### 构造方法
下面为HashMap的几个构造方法
```java
// 指定初始容量和装填因子
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))  // isNaN表示非法的浮点数，例如除数为0.0
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}
// 指定初始容量
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
}

public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}
```
默认的初始容量为`DEFAULT_INITIAL_CAPACITY = 1 << 4`，默认最大容量为`MAXIMUM_CAPACITY = 1 << 30`，默认装填因子为`DEFAULT_LOAD_FACTOR = 0.75f`。

### 存储数据
首先看HashMap的put方法：
```java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 判断Map是否为空
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```
