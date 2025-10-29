## 描述
* 将单向链表倒序

```python
# time: O(n) Space: O(1)
class Node:
	def __init__(self, value):
		self.value = value
		self.next = None

def reverse_link_list(head):
	prev = None
	cur = head
	while cur is not None:
		next = cur.next
		cur.next = prev
		prev = cur
		cur = next
	return prev
```


递归方式

```python

def reverse_list(head, prev=None):
	if head is None:
		return prev  # prev 是逆序以后的第一个元素
	next = head.next_node
	head.next_node = prev
	return reverse_list(next, head)

# 构造链表
node_a = Node('A')
node_b = Node('B')
node_c = Node('C')
node_d = Node('D')

node_a.next_node = node_b
node_b.next_node = node_c
node_c.next_node = node_d

# 逆序
ret = reverse_list(node_a)
# 打印逆序后的链表
while ret is not None:
	print(ret)
	ret = ret.next_node

```