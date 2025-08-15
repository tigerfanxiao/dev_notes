题目描述
在一个序列array中找到两个元素的和为target_sum

### 方法一 暴力方法

```python
# time: O(n^2)
# space: O(1)

def twoNumberSum(array, targetSum):
	for i in range(len(array) -1):
		first_num = array[i]
		# 不用去和前面的元素比较， 已经因为在上一次迭代时已经做过了
		for j in range(i + 1， len(array)): 
			second_num = array[j]
			if first_num + second_num == targetSum:
				return [first_num, second_num]
	return []
	
```


### 方法二 Hashtable
对比暴力的方法。 暴力的方法把序列遍历两次。第二次遍历本质上是查找 `target_sum - array[i]`。
我们把遍历过的元素放到Hash表后， 可以提升第二次遍历的速度。 


```python
def two_number_sum_hashtable(array, target_sum):
	num_read = {}
	for num in array:
		potentialMatch = target_sum - num
		if potentialMatch in num_read:
			return [num, potentialMatch]
		num_read[num] = True
	return []
```

### 方法三 排序后的使用左右指针
对序列排序复杂度 `O(nlogn)`
这种左右指针的思路有点像数学中逼进方法。 

```python
# time: O(nlog(n))
# space: O(1)
def two_number_sum_sort(array, targetSum):
	array.sort()
	left = 0
	right = len(array) - 1
	while left < right: 
		current_sum = array[left] + array[right]
		if  current_sum == targetSum:
			return [array[left], array[right]]
		elif current_sum < targetSum:
			left += 1
		else:
			right -= 1
	return []

```