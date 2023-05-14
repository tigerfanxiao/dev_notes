# 思维
1. 利用分而治之的想法, 将序列两两分拆. 
2. 分拆之后, 递归回来的时候, 需要将两个序列合并起来
3. 需要一种方法将两个序列合并


```python
# time O(nlogn)  space: O(nlogn)
def merge_sort(array):
	if len(array) == 1:
		return array
	mid = len(arr) // 2
	left = array[:mid]
	right = array[mid:]
	return merge(merge_sort(left), merge_sort(right))  
	

# assum left and right are ordered array
def merge(left, right):
	ordered_array = []
	i, j = 0, 0
	while i < len(left) and j < len(right):
		if left[i] <= right[j]:
			ordered_array.append(left[i])
			i += 1
		else:
			ordered_array.append(right[j])
			j += 1
	if left < len(left):
		ordered_array.extend(left[i:])
	if right < len(right):
		ordered_array.extend(right[j:])
	return ordered_array
	

```