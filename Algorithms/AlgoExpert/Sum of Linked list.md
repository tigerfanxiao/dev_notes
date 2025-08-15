# 描述

```shell

2 -> 4 -> 7 -> 1
9 -> 4 -> 5

# 把两个linked list加起来， 包括进位
2 -> 4 -> 7 -> 1
9 -> 4 -> 5
-----------------
1    9    2    2


```

# 解法

```python

class LinkedList:  
	def __init__(self, value):  
		self.value = value  
		self.next = None  
  
  
def traverse_linked_list(head):  
	if head is None:  
		return  
	print(head.value)  
	traverse_linked_list(head.next)  
  
  
def sum_of_linked_list(linked_list_one: LinkedList, linked_list_two: LinkedList):  
	new_linked_list_head = LinkedList(0)  
	current_node = new_linked_list_head  
	carry = 0  
  
	current_list_one = linked_list_one  
	current_list_two = linked_list_two  
  
while current_list_one is not None or current_list_two is not None or carry != 0:  
	value_one = current_list_one.value if current_list_one is not None else 0  
	value_two = current_list_two.value if current_list_two is not None else 0  
	sum_of_value = value_one + value_two + carry  
  
	node_value = sum_of_value % 10  
	carry = sum_of_value // 10  
	current_node.next = LinkedList(node_value)  
	current_node = current_node.next  
  
	current_list_one = current_list_one.next if current_list_one is not None else None  
	current_list_two = current_list_two.next if current_list_two is not None else None  
	  
	  
	return new_linked_list_head.next  
  
  
if __name__ == "__main__":  
	node_2 = LinkedList(2)  
	node_4 = LinkedList(4)  
	node_7 = LinkedList(7)  
	node_1 = LinkedList(1)  
	  
	node_2.next = node_4  
	node_4.next = node_7  
	node_7.next = node_1  
	  
	node_9 = LinkedList(9)  
	node_4 = LinkedList(4)  
	node_5 = LinkedList(5)  
	  
	node_9.next = node_4  
	node_4.next = node_5  
	  
	sum_of_node = sum_of_linked_list(node_2, node_9)  
	traverse_linked_list(sum_of_node)

```
