和冒泡排序的区别在于, 冒泡排序是邻接的两个元素交换位置, 把最大的元素推到最右边. 选择排序是始终和最左侧的元素调换位置. 

# 思维
* 把子序列中最小的元素移动到最左侧, 用 i 控制
* 每次迭代 j, 都和当前最小元素 i 做对比, 如果发现有更小的元素, 就调换位置. 
* 始终保持最小值, 在当前子序列的最左侧. 

```python
# time O(n^2) Space O(N)

def selectionSort(arr):    
	for i in range(len(arr) - 1):             
		for j in range(i + 1, len(arr)):            
			if arr[j] < arr[i]:                
				arr[i], arr[j] = arr[j], arr[i] 
	return arr
	
my_arr = [5,3,2,8] 
selectionSort(my_arr)
```