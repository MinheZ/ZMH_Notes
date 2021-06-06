* [1 MySQL 索引数据结构](#1-MySQL-索引数据结构)
* [2 当查询定位到叶子结点之后怎么查找对应元素](#2-当查询定位到叶子结点之后怎么查找对应元素)
* [3 MySQL 查询优化](#3-MySQL-查询优化)
* [4 索引查数据需要查几次数据库](#4-索引查数据需要查几次数据库)

----------------

### [1 MySQL 索引数据结构](https://github.com/MinheZ/Notes/blob/master/note/MySQL.md#%E4%B8%80%E7%B4%A2%E5%BC%95)

### 2 当查询定位到叶子结点之后怎么查找对应元素
每个叶子结点在磁盘上占据的空间为 1 页，且这 1 页的数据是有序的，因此用二分查找就行。

### [3 MySQL 查询优化](https://github.com/MinheZ/Notes/blob/master/note/MySQL.md#%E4%BA%8C%E6%9F%A5%E8%AF%A2%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)

### 4 索引查数据需要查几次数据库
