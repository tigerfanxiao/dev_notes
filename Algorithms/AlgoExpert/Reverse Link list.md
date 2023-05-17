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
	return cur
```

