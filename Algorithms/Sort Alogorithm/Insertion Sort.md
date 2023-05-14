# 思维
* 在这种算法中引入了排序序列
* 假设是一个排序好的序列, 要插入一个新的元素. 只要将新元素从最后一个元素开始比较, 通过逐个两两比较, 调换位置, 直到找到第一个比自己小的元素, 就停止
* 我们把第一个元素看做是只有一个元素的有序序列, 第二个元素要插入这个序列中


```python
# time: O(n^2) space O(N)

def insertion_sort(array): 
	for i in range(1, len(array)  # 新元素从第二个元素开始
		while i > 0 and array[i-1] > array[i]:
			array[i-1], array[i] = array[i], array[i-1]
			i -= 1
	return array
```