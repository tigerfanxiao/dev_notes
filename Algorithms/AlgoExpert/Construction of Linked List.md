
双向链表
```python
class Node:  
	def __init__(self, value):  
		self.value = value  
		self.prev = None  
		self.next = None  
	  
	def __repr__(self):  
		return self.value  
	  
  
class DoublyLinkedList:  
	def __init__(self):  
		self.head = None  
		self.tail = None  
  
	def append_node(self, node: Node):  
	# to the end of list  
		if self.head is None:  
		self.head = node  
		elif self.head is not None:  
		self.tail.next = node  
		node.prev = self.tail  
		self.tail = node  
		return  
  
	def __repr__(self):  
		values = []  
		cur = self.head  
		while cur is not None:  
			values.append(cur.value)  
			cur = cur.next  
		return ", ".join(values)  
	  
	def setHead(self, node):  
		# node 是链表中的元素  
		if self.head is None:  
			self.head = node  
			self.tail = node  
		return  
		self.insertBefore(self.head, node)  
  
	def setTail(self, node):  
		if self.tail is None:  
			self.setHead(node)  
			return  
		self.insertAfter(self.tail, node)  
  
	def insertBefore(self, node, node_to_insert):  
	# 把链表中的元素插入到链表另一个地方, 如果  
		if node_to_insert == self.head and node_to_insert == self.tail:  
			return  
		if node == node_to_insert:  
			return  
		self.remove(node_to_insert)  
		node_to_insert.prev = node.prev  
		node_to_insert.next = node  
		if node.prev is None:  
			self.head = node_to_insert  
		else:  
			node.prev.next = node_to_insert  
			node.prev = node_to_insert  
	  
	def insertAfter(self, node, node_to_insert):  
		if node_to_insert == self.head and node_to_insert == self.tail:  
			return  
		self.remove(node_to_insert)  
		node_to_insert.prev = node  
		node_to_insert.next = node.next  
		if node.next is None:  
			self.tail = node_to_insert  
		else:  
			node.next.prev = node_to_insert  
			node.next = node_to_insert  
	  
	def find_node_position(self, node):  
		cur = self.head  
		postion = 1  
		while cur is not None and node != cur:  
			cur = cur.next  
			postion += 1  
		return postion  
		
	def insertAtPosition(self, position, node_to_insert):  
	# node_to_insert 是链表中的元素  
		if position == 1:  
			self.setHead(node_to_insert)  
			return  
		node = self.head  
		current_position = 1  
		while node is not None and current_position != position:  
			node = node.next  
			current_position += 1  
		if node is not None:  
			# 如果是 node_to_insert 在 position后， insertBefore
			if self.find_node_position(node_to_insert) > position:  
				self.insertBefore(node, node_to_insert) 
			# 如果是 node_to_insert 在 position前， insertAfter 
			else:  
				self.insertAfter(node, node_to_insert)  
		else: # 插在最后
			self.setTail(node_to_insert)  
  
	def removeNodesWithValue(self, value):  
		cur = self.head  
		while cur is not None:  
			node_to_remove = cur  
			cur = cur.next  
			if node_to_remove.value == value:  
				self.remove(node_to_remove)  
	  
	def remove(self, node):  
		if node == self.head:  
			self.head = self.head.next  
		if node == self.tail:  
			self.tail = self.tail.prev  
		self.removeNodeBindings(node)  
  
	def removeNodeBindings(self, node):  
		if node.prev is not None:  
			node.prev.next = node.next  
		if node.next is not None:  
			node.next.prev = node.prev  
		node.prev = None  
		node.next = None  
	  
	def contains_node_with_value(self, value):  
		cur_node = self.head  
		while cur_node is not None and cur_node.value != value:  
			cur_node = cur_node.next  
		return cur_node is not None  
	  
  
if __name__ == '__main__':  
	node_a = Node('a')  
	node_b = Node('b')  
	node_c = Node('c')  
	node_d = Node('d')  
	node_e = Node('e')  
	node_f = Node('f')  
	double_liked_list = DoublyLinkedList()  
	double_liked_list.append_node(node_a)  
	double_liked_list.append_node(node_b)  
	double_liked_list.append_node(node_c)  
	double_liked_list.append_node(node_d)  
	double_liked_list.append_node(node_e)  
	double_liked_list.append_node(node_f)  
  
	# ret = double_liked_list.containsNodeWithValue('a')  
	# double_liked_list.remove(node_b)  
	print(double_liked_list)  
	print(double_liked_list.head)  
	print(double_liked_list.tail)  
	print(double_liked_list.contains_node_with_value('a'))  
	# double_liked_list.remove(node_d)  
	# print(double_liked_list)  
	  
	# double_liked_list.removeNodesWithValue('a')  
	# print(double_liked_list)  
	  
	# double_liked_list.insertBefore(node_a, node_b)  
	# print(double_liked_list)  
	  
	# double_liked_list.insertAfter(node_e, node_b)  
	# print(double_liked_list)  
	  
	double_liked_list.insertAtPosition(4, node_a)  
	print(double_liked_list)
```