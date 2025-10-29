# 描述

1. 将序列的前半段和后半段的逆序，组合成一个新的序列
```
1 -> 2 -> 3 ->4 -> 5 -> 6

# 转变位
1 -> 6 -> 2 -> 5 -> 3 -> 4
```

# 解法1

```python

class Node:  
	def __init__(self, value):  
	self.value = value  
	self.next = None  
  
def __repr__(self):  
	return self.value  
  
  
def traverse_linked_list(head):  
	while head is not None:  
	print(head.value)  
	head = head.next  
	  
  
def split_linked_list(head):  
	slow_pointer = head  
	fast_pointer = head  
	  
	while fast_pointer is not None and fast_pointer.next is not None:  
		slow_pointer = slow_pointer.next  
		fast_pointer = fast_pointer.next.next  
		  
	second_half_list = slow_pointer.next  
	slow_pointer.next = None  
	return second_half_list  
  
  
def reverse_linked_list(head):  
	prev = None  
	cur = head  
	while cur is not None:  
		next = cur.next  
		cur.next = prev  
		prev = cur  
		cur = next  
	return prev  
	  
  
def interweave_link_list(head1, head2):  
	cur1 = head1  
	cur2 = head2  
	while cur1 is not None and cur2 is not None:  
		next1 = cur1.next  
		cur1.next = cur2  
		next2 = cur2.next  
		cur2.next = next1  
	  
		cur1 = next1  
		cur2 = next2  
	return head1  
	  
  
def zip_link_list(head):  
	second_half_list = split_linked_list(head)  
	second_half_list = reverse_linked_list(second_half_list)  
	return interweave_link_list(head, second_half_list)  
  
  
if __name__ == "__main__":  
	node_1 = Node("1")  
	node_2 = Node("2")  
	node_3 = Node("3")  
	node_4 = Node("4")  
	node_5 = Node("5")  
	node_6 = Node("6")  
	node_1.next = node_2  
	node_2.next = node_3  
	node_3.next = node_4  
	node_4.next = node_5  
	node_5.next = node_6  
	  
  
  
new_list = zip_link_list(node_1)  
traverse_linked_list(new_list)

```