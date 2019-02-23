* [概述](#概述)
* [数据类型](#数据类型)
* [数据结构](#数据结构)

----------------------------------

# 概述
Redis是一种速度非常快的非关系型(NoSQL)高性能键值对(key-value)数据库。它通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
- 字符串(String)
- 列表(List)
- 集合(Set)
- 散列表(Hash)
- 有序集合(ZSet)

其应用场景也非常广泛：
- 缓存(最多)。
- 分布式集群架构中的session分离。
- 任务队列等。

-------------------------------

# 数据类型
关于Redis的数据类型，此文章有详细讲述：[what redis data structure look like?](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/)

# 数据结构
## 字典
dictht是一个散列表的结构，使用的是链地址法处理哈希冲突。
```c
/* This is our hash table structure. Every dictionary has two of this as we
 * implement incremental rehashing, for the old to the new table. */
typedef struct dictht {
    dictEntry **table;
    unsigned long size;
    unsigned long sizemask;
    unsigned long used;
} dictht;
```
```c
typedef struct dictEntry {
    void *key;
    union {
        void *val;
        uint64_t u64;
        int64_t s64;
        double d;
    } v;
    struct dictEntry *next;
} dictEntry;
```
Redis的字典dict中，含有2个dictht，这是为了方便rehash操作。在扩容时，将其中一个dictht上的键值对rehash到另外一个字典中去，完成后释放空间，并且互换两张表的角色。
```c
typedef struct dict {
    dictType *type;
    void *privdata;
    dictht ht[2];
    long rehashidx;          /* rehashing not in progress if rehashidx == -1 */
    unsigned long iterators; /* number of iterators currently running */
} dict;
```
rehash操作是采用渐进方式，这是为了避免一次性执行过多的rehash给服务器带来过大的负担。

渐进式rehash通过记录dict的rehashidx完成。它从0开始，然后每一次rehash都会递增。

例如把`dict[0]`上的`table[rehashidx]`键值对rehash到`dict[1]`上，`dict[0]`的`table[rehashidx]`变为`null`，然后执行`rehashidx++`。

在rehash期间，每进行一次增删改查操作，都会执行一次渐进式的rehash操作。采用渐进式的rehash会导致字典中的数据分散在2个dictht上，因此对字典的查找操作也要到对应的dictht上去执行。
