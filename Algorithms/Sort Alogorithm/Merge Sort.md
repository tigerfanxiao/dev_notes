# 思维
1. 利用分而治之的想法, 将序列两两分拆. 
2. 分拆之后, 递归回来的时候, 需要将两个序列合并起来
3. 需要一种方法将两个序列合并
4. 这里的merge 函数不是递归算法. 递归的是 merge_sort 函数


```python
# time O(nlogn)  space: O(nlogn)
def merge_sort(array):
	if len(array) == 1:
		return array
	mid = len(arr) // 2
	left = array[:mid]
	right = array[mid:]
# merge是应用在拍好许多序列中, 所以用merge_sort(left) 来保证在递归中, 左右子序列已经排序
	return merge_helper(merge_sort(left), merge_sort(right))  
	

# assum left and right are ordered array
def merge_helper(left, right):
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