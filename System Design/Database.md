

系统设计没有标准答案,
只有优点和缺点

系统设计和算法设计的区别
算法设计追求的是绝对精确，系统设计是追求的是基本可用
系统设计可以追求99%的用户可用，1%通过一条调整来解决

## 4S分析法
Scenario
Service
Storage
Scale 升级优化


# 数据库
一般主流的公司SQL和NoSQL都会用到， 以面对不同的应用场景

## 数据库原理 

修改数据的思维实验
1. 直接修改文件中的一行
2. 读取整个文件， 修改好了， 覆盖重新写入
3. 不修改， 直接append操作追加在文件后面
回答：
1. 很难做到直接修改文件内容给， 原来是4个字节， 如果现在修改为8个字节， 后面的内容需要进行移位
2. 如果是读取整个文件， 重新写入覆盖原文件，则非常耗费时间， 每次读取写入其实其他内容不变
3. append操作是比较合理的。 增加一个时间戳，就知道谁是最后写入的。读取数据的时候前面的数据是排序的， 后面新增的数据是无序的
我们在写的时候是分块有序，在最后一块无序


## 怎么选择SQL还是NoSQL
* 首先要判断是数据库是写操作多还是读操作多
SQL更适合读操作多的数据库， NoSQL数据库比如Redis适合写操作比较多的数据库
* QPS的要求
QPS对比表
|数据库|QPS||
|--|--|--|
|Cassandra|100||

NoSQL数据库比如Redis的QPS要比SQL数据库高一个量级
* 是否要做复杂查询， 或者多维度查询
SQL数据库更适合做多维度的查询，在不同的列上定义索引。 NoSQL数据库更适合做简单的键值对的查询


## 怎么选择Redis和Memcached
* 数据是否需要持久化
Redis支持数据持久化， Memcached不支持持久化
* 数据结构的多样性
Redis支持多种数据结构， 比如list， set，但Memcached只有string结构


## 怎么选择Redis和Cassandra

* Cassandra相对Redis要慢， 平均每台机器在100QPS， 但是稳定性更高，有多个Replica。 需要使用更多的机器来提升QPS
* Redis的写入速度快，但是有单点故障。为了避免单点故障，可以考虑Cassandra






