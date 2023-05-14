
LRU = Least Recently Used 最少被访问的元素
获取在内存中， 最近刚被访问过的key，
需求： 
1. Cache是有固定的Size的，假设等于3
2. 如果Cache中要增加一个新的元素，此时Cache没有满。新元素就存到Cache中， 放在队列的顶端
3. 如果Cache中的某一个元素被访问了，就把这个元素放到队列的顶端
4. 如果Cache中要增加一个新的元素，此时Cache已经满了，把最少被访问的元素去掉， 队列的最后一个元素。 把新的元素放在队列的顶端
5. 如果要对Cache中的某一个元素赋一个新值，首先从cache中找到这个值，然后附上新值，再把这个key放到队列的顶端

## 数据结构的选择
1. Hashmap
	Hashmap在取数据的时候很快，但是如果想要获取最近被访问的元素， 需要给每个key增加一个时间戳，而且我们也没法按照时间戳获取元素
2. Array
	如果一个元素被访问，我们要把这个元素放到队列的头部。这个操作对Array来说非常耗时O(n), 所以我们想到用Linked List
3. Linked List
	Linked List可以方便的调换元素的位置，但是查询元素需要遍历整个list。所以考虑把Hashmap和linked list的特性组合起来。用Hashmap保存每个node的地址，node之间的指向来保证排列的顺序

Cache应该是FIFO

  
LRU的使用场景学习

如果LRU Cache size = 3

 