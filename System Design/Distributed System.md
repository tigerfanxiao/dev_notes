# 三大问题
1. 文件系统
2. 计算
3. 数据库

# 文件系统
痛点
1. 存储不够
2. QPS太大
本质上就是用多台机器解决一台机器解决不了的问题
历史上在解决这个问题上有两派公司: google和Sun


google的方式是用很多便宜的机器来处理
	1. 产生的问题: 便宜的机器容易坏, 需要频繁的更换机器

sun 多年前卖给了Oracle, java就是他们搞的
1. Sun用大型机来解决这个问题, 应该是失败了, 因为公司倒闭被收购了

google三剑客
1. 分布式文件系统Distributed File System (GFS Google File System )
	怎么存储数据?
	No SQL 底层需要一个文件系统
2. Map Reduce
   怎么快速处理数据?
3. BigTable = No-SQL Database
   怎样连接底层和上层数据

分布文件系统怎么用
一些相对规模比较小的网站, 比如Airbnb会使用AWS或者GCP的服务, 而不是自己搭建分布式文件系统. 对于大公司, google和microsoft就会构建自己的分布式文件系统, 会对自己的对立业务做优化
我们需要怎样理解Failure和Recovery

首先分布式文件系统是google搞出来的(用C++实现), google的工程师去了yahoo, 用java实现了分布式文件系统
HDFS Yahoo(Alibaba) Open Source of GFS, 全程是Hadoop Distributed File System
创在文件系统更多目的是为了加快计算. 如何加快计算, 就使用了Map Reduce

gfs 是分布式系统做存储用的
Webserver - DB -  GFS
Webserver - GFS
如何设计文件系统, 需求有哪些
需求一
1. 用户写入一个文件, 用户读取一个文件
2. 支持多大的文件?
	1. 越大越好? 比如 > 1000T

需求2
1. 多少台机器存储这些文件
2. 支持多少台机器
	1. 越多越好? 2007年 google用10万台机器存储
问题
如何保证数据的一致性


Service服务
Client +Server
这里的Client是指Webserver和DB
这里的Server是指多台机器. 这就牵涉到多台机器之间的沟通问题
* Peer to Peer (Peer之间平等)
*  Master 和 Slave server. Client只要和Master联系
P2P的问题在于多台设备数据同步问题, Master Slave的模式, Master知道文件存在哪个Slave Server上. 

在GFS上, Slave不是Master的备份. Master是管理者(不存文件), Slave是执行者存文件
在数据库的语境中Slave是backup, 在master被干掉后, slave可以充当master

Facebook 的Cassandra和BitComent就是P2P模式
优点, 一台机器挂了还可以工作
缺点, 多台机器需要经常保持数据一致

Master Slave
优点: 设计简单, 数据容易保持一致
缺点: 单master要挂
最后google决定使用Master Slave模式

什么是数据一致性问题?
如果在很短的时间内, 收到多个修改数据的请求, 在一台服务器修改的同时, 第二台服务器也接到了修改请求, 那么怎么保证这个被修改的数据在两台服务器上是一致的, 同一个值, 如果只有一个Master, 那么由Master来决定存哪个值, 如果是P2P, 则需要通过协商(设计网络通信)来决定


存储
大文件存在哪里? 内存, 数据库, 还是文件系统
大文件是直接存在文件系统上....的, 而不是存在数据库里, 因为数据库里更适合存结构化之后的数据. 

怎么存在文件系统里面?
操作系统怎么存文件?
1. 文件内容
2. meta data
	1. 文件名
	2. 文件写入和修改的时间
	3. 文件的tag
	4. 文件的格式, 大小
	5. 在linux中Index索引就是文件每一小块在磁盘中的文件
一个问题是文件的内容和文件的信息是分开存好还是放在一起存好
答案是分开存好: 因为meta data的访问次数远远大于文件内容访问的次数
而且meta data会在内存和硬盘上同时保存. 但是文件内容是在硬盘上

下一个问题是一个大文件是单个存储还是分割后分开存储?
答案: windows选择是连续, Linux选择的是分开
windows连续存储, 就会有很多磁盘碎片. 长久使用后需要磁盘碎片化整理
Linux可能是以1K为一个小块将文件分拆. 缺点是需要维护文件分割的存储表. 在取文件的时候按照指针来把小块结合起来


下一个问题: 如果一个文件太大, 在一台设备上存不下, 怎么办?
如果是一个100T文件
```
100 *1000G
= 100 * 1000 * 1000 * 1000 K
如果1K是一个block, 那么meta index就要存
100 * 1000 * 1000 * 1000 block(chunk)的地址
```
这时, 如果一个指针的字节是4个字节, 那还是太多了. 为了meta index的信息不那么大. 
就把 1K的分块大小变成64M, 缺点是对于小文件的话, 就会有浪费. 
选择64M的运营是通过实验得出的比较平衡的

google使用的是的Master Slave架构. 在master设备上保存了meta data, 文件的某一个chunk存在在哪一台机器上

下一个问题: 为什么不用多种chunk的方案, 比如将文件分为两类, 大文件为64M为一个chunk, 小文件用1K为chunk?
答案: 主要是同一种chunk尺寸, 在计算偏移位上非常方便, 加快寻址效率

下一个问题: Master上的metadata是不是存offset信息(偏移量)?
答案: 在master上不需要存offset信息, 为了减轻master的负担, 我们只要在master上找到文件的chunk所在的机器, 然后到具体的机器上去看机器的偏移量就行. 也就是说在slave机器上保存了一个chunk和磁盘位置的表. 通过Slave上的这张表才最后找到了文件内容chunk. 这样做的好处还有, 如果slave机器出问题了, 或者磁盘更换了, 只要在slave上做变更就行, 不需要再通知master

下一个问题. 如果一个文件10P大, 那他的meta data能存进内存吗?
答案: 1 chunk = 64 MB needs 65 B (经验值) 的meta data
```
10P = 16*10^6 chunk needs 10G 内存来存 meta data
```

下一个问题: 怎么写一个文件? 如果有一个大文件, 是先直接写入, 还是碎尸后写入?
答案: 一次写入和多次写入的优缺点. 一次写入过程中, 如果文件写错了, 比如网络中断了, 就不得不重新传输. 这样保证中断重传. 

如果分成多份多次写入, 那么每一份的大小?
回答: 文件本来就是按照chunk来存储的, 所以传输单位也是chunk

如果是分成多次写入, 那么是master来切分还是client来切分
回答: 应该是client来碎尸, 在传输之前就切碎了
首先浏览器已经将文件切割了, 把chuck发给webserver, 
webserver或者是database, 往文件系统上去写

每一个chunk是怎么写入server的呢?
需要先跟master沟通, master分配一个chunkserver地址, 再由client直接到chunkserver上去写
master 的分配方式有很多, 比如查看活最少的, 空间比较多的

下一个问题: 如果我要修改某一个chunk的内容, 但是这chunk的内容变化了, 要怎么办?
gfs为了保证高效, 是不支持修改操作的, 需要修改的地方是重新写一个新的, 然后删除旧的

下一个问题: 一次读入整个文件, 还是多次读取
先和master沟通, 知道chunk存在哪个chunk server上, 然后直接取chunk server上去取

下一个问题: 为什么不让master去往chunk server上去写?
答案: master会成bottle neck, 承载的任务太多了





# Master Slave架构的优化问题
下一个问题: 
单master够不够, 工业界90%的系统都采用单master, simple is perfect
如果用两个master, 一个master不断的向另一个master同步, 这是hadoop用的方法

下一个问题:
因为我们用了很多很便宜的机器, 容易出错. 我们怎么发现哪个chunk出错了?
回答: checksum, 比如MD5. 把checksum放到chunk的最末尾
一般情况下一个checksum = 4 bytes = 32bit 一个整数的大小
1P的文件, 大概有 1P/64MB * 32 bit = 62.5 MB大小的checksum

下一个问题: 什么时候检查checksum?
1. 读数据的时候会检查
2. 周期性也会检查. 周期性的修复

下一个问题: 
如何在chunkserver挂了之前避免chunk数据丢回
答案: Replica. 一份数据不只是存一份, 而是存3份. 
下一个问题: 存三份都存在哪里? 一般是两份存同一个region, 或者存一个AZ, 第一份存另外一个region. 两份存在一起是因为, 发现一份错了之后, 立刻能取到备用的

下一个问题:
master选择Chunk Server的策略
1. 最近写入比较少 LRU (建议做一下这个算法题)
2. 磁盘存储比较低

下一个问题
How to recover when is chunk is lost?
答案: 因为一个chunk被保存为3份, 如果一份坏了, 我们就问master那里有好的备份. 然后去好的备份上去取数据

下一个问题 How to find whether a chunkserver is down?
Heartbeat: 使用心跳 master 不断去听ChunkServer的心跳
Heartbeat有两种: 
1. Master向Server发
2. Server向Master发. 应该是server向Master发比较好. 减少master的负担

下一个问题
一般工作中, master管理多少个chunkserver
答案: 一般是1k-2K台

下一个问题 因为要保存三个备份, 是不是client每次会连通三台机器, 写三台机器
答案: master给出三个chunkServer时, 会指定一个server为队长, client向队长写入. 然后由队长向剩下的2台机器写入. 
下一个问题: 队长怎么选? 
1. 选离client近的
2. 写入比较少
3. 磁盘利用率低

下一个问题: client向chuckserver1中写入的时候中断了会不会造成磁盘碎片
chuckserver有缓存器, 一般等待一个chunk收到完整之后, 向硬盘上再去写. 所以这种情况不会造成硬盘碎片.

下一个问题: 如果写的过程中, 其中一个chunk Server挂了怎么办?
client要retry, 此时master可能因为长久没有收到chunkserver的心跳. master会重新从可用的chunkserver中选3个server

在HDFS中
master叫name node, chunk server 叫data node

GFS的创在是为了MapReduce所用. 一般是为了高输出量的场景中用

 
# Map Reduce
首先gfs是做分布式系统的存储用的, Map Reduce是做计算用的. 
问题: count the word frequecy of a web page?
如果是单机处理的情况, 就是通过构建hash表来实现. 时间复杂度O(n)
如果还要在提升, 就是要多机处理
在多机处理的过程中的瓶颈是汇总太慢的问题,  map reduce在汇总的时候做了并行处理, 也就是说 a,b的汇总在一台机子上, c,d的汇总在另一台机子上. 
最后再做一个总体的汇总. 在最后的汇总时, 实际上并不是合成一个文件. 而是在一个文件夹下, 保存了多个文件, 只有在最后使用的时候, 才合并. 

Map是有框架的
Map Reduce步骤
1. Input 
2. Split 
3. Map
4. parttition sort
5. Fetch +Merge Sort
6. Reduce
7. Output
注: 第4和第5步合起来成为shuffle 洗牌 , 有一道lintcode题 lnverted index (map reduce)
Inverted Index 倒排索引
正向索引: 比如图书馆的编号
0 对应 哈利波特 包含单词 Car, River
1 对应 海贼王 包含单词 海贼王, dear
2 对应小红帽 包含单词 cat, car
当我要查询"海贼王"这个单词出现在哪一本书里的时候. 会先进行倒排缩影
统计 car ( 0, 2) 即car这个单词出现在 哈利波特和小红帽中
统计 river (0) 即river这个单词出现在哈利波特中
统计海贼王(1) 即海贼王这个单词出现在海贼王这本书中
可以看到这个倒排索引很耗时, 所以类似google这样的网站是预先建立好倒排索引的

查看一个算法 Flatten nested list iterator理解iterator
Anagram 用 mapreduce来做



在mapreduce中避免开数据结构比较大的hashtable
在map的时候, 只要发现一个就对reduce的服务器进行传输. 而不是等到统计完了再做传输, 因为传输可能是网络耗时

在mapReduce中需要实现的只有map函数和reduce函数
map是输入是key value形式

google在做全球的热点词汇的时候用来多少台设备?
全球查询的词汇是10PB, 做了1000map, 1000reduce的规模. 这个规模是通过测试得到的. 
是不是机器越多就越好?
答案: 机器越多, 处理的速度越快. 缺点时, 机器越多, 启动的时间越长. 
如果不考虑启动时间, Reduce的机器越多越开吗? 
答案: key的数目就是reduce的上限. 同时google会做一个stop list, 就是对一些没有实际含义的单词不做处理


传输整理的过程
1. 单词排序了
	1. 这里用快速排序 Fast Sort, Bucket Sort, Counting Sort都不够好(需要大内存). 因为上面这些排序是内存排序. 也成为内排序. 但是在大数据场景上, 单台机器的内存不够大. 
	2. 所以需要使用外排序. 在map在内部, 做一个partition操作. 对小部分先sort, 在用merge sort排序 (merge k sort list/array 也成为k路归并, 有基于内存版本的, 也有机遇硬盘版本的) 
2. 外排序:  k路归并怎么做外排序? 外排序会比较慢, 但是保证了可操作性





问题: 为什么要统计所有网站的词频?
微博可以统计每日最火热的话题. 


遗留问题: 为什么Hashtable的查询时间是O(1)?

怎么设计MapReduce系统
gfs肯定比本地磁盘慢, 需要做replica, 存三份, 还要做service

运行过程中, mapper和reducer挂了怎么办? 是重启一台机器, 还是重做?
会从worker池子中去出部分, 如果有worker挂了, 就从新取一个备胎出来. 

如果reducer一个机器上key特别大怎么办, 比如如要统计一个网站的流量
比如facebook 我想要统计facebook网站上出现facebook这个单词的url有多少个. 
可能的情况是, 我有1000个单词需要统计, 999个单词已经完成了统计, 但是因为facebook这个单词出现次数单词, 这个key的统计迟迟没有跑完. 这种情况怎么办
生产环境中, mapreduce一般跑2-5小时, 超过这个时限, 就需要debug和优化
一个解决方法是, 在facebook这个单词后加一个1到10的随机数字, 这样就会创造成多个key, 这样facebook会被平分到10台机器上. 不会使某一台机器负载特别大, 在output时再把facebook的不同的结果合在一起

问题: input 和output应该是放在gfs上
问题: Local



# Big Table
分布式系统的数据库

BigTable也是NoSQL数据库
只不过是google开发的。 
Hbase yahoo(Alitaba) 开源的bigTable, 在hadoop生态圈
Cassandra Facebook开发 差不多是开源的
DynamoDB Amazon 开发

题引
为什么会有数据库， 因为如果是如果要查询文件中的某一个字段。需要便利整个文件

Big Table如何实现

怎么把多个有序块合并为一个有序块: k路归并, K Merge Sort

有没有做过 tree serialization这个题

数据库最后的一个分块是在内存中的, 排序后再写入硬盘. 
怎样避免内存挂了情况, 使用Write Ahead Log
bloom filter : False is always False, True maybe True
采用bloom filter的原因是只要存储bit数组, 不需要存key, 比hash表节省了空间


读写同时发生
bigtable有race condition, 因为bigtable有修改
为何gfs没有race condition, 因为gfs没有修改
读写冲突的时候, 就需要有分布式锁zookeeper(hadoop)或chubby(google)
锁 lock server 有consistent hash map , 有锁服务器之后, meta data就放在锁服务器上, master只要管理slave, 检测slave, 保证slave运转健康

client写入的过程
client先访问锁服务器,  锁把对应的row key 锁住, 并告诉client那台机器已经锁好了. client访问tablet server, 告诉key value值告诉他, tablet server把key value写成write log, 并把数据接入本地的内存和硬盘. 然后再把数据写到GFS中

client读的过程
client先访问锁服务器, 锁把对应的row key锁住, 
到tablet server1 上会先检查内存, 如果已经在内存中了, 就返回. 如果不在内存中, 则检查Bloom filter on eash sstable, 检查index是不是在sstable中. 这样可以快速的判定, 数据在哪一个sstable上. 然后根据sstable到gfs上映射. 然后到gfs上去运用binary Search读取value

数据库和文件系统的区别是什么
1. 数据库存储的对象是key value, 而文件系统处理的对象是文件

### 总结
Design
	Client + Master + Tablet Server + Distributed Lock
Client
	Read + Write
Tablet Server
	Maintain the Data (Key Value pairs)
Master
	 Shard the file
	 Manage the servers health
Distributed Lock
	Update MetaData
	Main the MetaData
	Lock key
上面这个设计没有考虑缓存, 实际上不是每一次都需要访问数据库

特性
write append  加快写操作
Sstable 排序表的形式来存储

读
Binary Search on disk 硬盘上二分操作
Index 不是每个key 都检测
Bloom filter