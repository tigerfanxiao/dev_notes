
# 思路

考虑 [[Two Number Sum]] 中的左右指针的解法。 当我们在选中序列中的一个元素时，这个元素和targetSum的差， 对余下的序列做 two number sum.


```python
# time: O(n^2)
# space: O(N)
def threeNumberSum(array, targetSum):
	array.sort()
	triplets = []
	
	for i in range(len(array) - 2):
		left = i + 1
		right = len(array) - 1
		while left < right:
			current_sum = array[i] + array[left] + array[right]
			if  current_sum == targetSum:
				triplets.append([array[i], array[left], array[right]])
				# 找到结果， 两边同时缩进
				left += 1  
				right -= 1
			elif current_sum < target_sum:
				left +=1
			else: 
				right -= 1
			
	return triplets 
```