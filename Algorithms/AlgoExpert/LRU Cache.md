
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

```shell
# step 1 初始装填

    c -> 3  # c 是最近被访问的
    b -> 2
    a -> 1  # 此时 a 是栈底位 最少被访问的

# step 2 a 被访问了
	a -> 1
	c -> 3
	b -> 2

# step 3 加了 3 的元素 d -> 4
	d -> 4
	a -> 1
	c -> 3
    # 原先的 b ->2 被弹栈了

# step 4 a 的值被修改为 5
	a -> 5
	d -> 4
	c -> 3

```

数据结构
```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

    def removeBindings(self):
        if self.prev is not None:
            self.prev.next = self.next
        if self.next is not None:
            self.next.prev = self.prev
        self.prev = None
        self.next = None


class DeoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def setHeadTo(self, node):
        if self.head == node: 
            return # 如果 node 已经是 head 了  
        elif self.head is None: # 链表为空
            self.head = node
            self.tail = node 
        elif self.head == self.tail: # 链表只有一个元素
            self.tail.prev = node 
            self.head = node # 插在前面
            self.head.next = self.tail 
        else: # 把链表中间的元素设置为 head
            if self.tail == node: # 如果是把 tail 元素设置为 head
                self.removeTail()
            node.removeBindings()
            self.head.prev = node
            node.next = self.head
            self.head = node
            

    def removeTail(self):
        if self.tail is None:
            return 
        if self.tail == self.head:  # 如果只有一个元素
            self.head = None
            self.tail = None

        self.tail = self.tail.prev
        self.tail.next = None

    def __repr__(self) -> str:
        nodes = []
        cur = self.head
        while cur is not None:
            nodes.append(f'{cur.key} -> {cur.value}')
            cur = cur.next
        return "\n".join(nodes)


class LRUCache:
    def __init__(self, maxSize=1):
        self.maxSize = maxSize
        self.listOfMostRecent = DeoublyLinkedList()
        self.cache = {}
        self.currentSize = 0
    
    def __repr__(self) -> str:
        return str(self.listOfMostRecent)


    # time: O(1) space O(1)
    def insertKeyValuePair(self, key, value):
        if key not in self.cache:
            if self.currentSize == self.maxSize:
                self.evictLeastRecent()
            else:
                self.currentSize += 1
            self.cache[key] = Node(key, value)
        else:
            self.replaceKey(key, value)
        self.updateMostRecent(self.cache[key])

    def updateMostRecent(self, node):
        self.listOfMostRecent.setHeadTo(node)

    def getValueFromKey(self, key):
        if key not in self.cache:
            return None
        self.updateMostRecent(self.cache[key])
        return self.cache[key].value

    def getMostRecentKey(self):
        return self.listOfMostRecent.head.key

    def replaceKey(self, key, value):
        if key not in self.cache:
            raise Exception("The provided key isn't in the cache!")
        self.cache[key].value = value
    
    def evictLeastRecent(self):
        keyToRemove = self.listOfMostRecent.tail.key
        self.listOfMostRecent.removeTail()
        del self.cache[keyToRemove]  # delete in cache dict

lru_cache = LRUCache(3)

print('Step 1: init cache')
lru_cache.insertKeyValuePair('a', 1)
lru_cache.insertKeyValuePair('b', 2)
lru_cache.insertKeyValuePair('c', 3)
print(lru_cache)

print('Step 2: visit least recent key a')
lru_cache.getValueFromKey('a')
print(lru_cache)

print('Step 3: add new key value pair d -> 4')
lru_cache.insertKeyValuePair('d', 4)
print(lru_cache)

print('Step 4: Change the value of existing key a to 5')
lru_cache.insertKeyValuePair('a', 5)
print(lru_cache)
```