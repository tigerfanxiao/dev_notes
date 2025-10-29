# 思维
1. 本质上也是使用了分而治之的方法
2. 但是和 merge sort 不同的地方是, 并不是合并两个有序的序列. 而是通过一个 pivot 值, 将序列分为比 pivot 小的子序列和比 pivot 大的序列. 
3. merge sort在分拆的时候没有花力气, quick sort 在分拆的时候, 就使用 pivot 分拆出小于 pivot 和大于 pivot 的两个序列. 所以在合并的时候, 就不用花力气了

### 方法一

```python
def quick_sort(arr):
	if len(arr) <= 1:
		return arr
	left, right = [], []
	pivot = arr.pop()
	for num in arr:
		if num <= pivot:
			left.append(num)
		else:
			right.append(num)
	return quick_sort(left) + [pivot] + quick_sort(right)

```

### 方法二

* 对比方法一, 节约了空间. 因为没有构建新的序列, 而是在原有的序列上不断交换元素位置
* 注意学习带有序列作为参数的递归写法

```python
def quick_sort(array):
    quick_sort_helper(array, 0, len(array) - 1)
    return array

def quick_sort_helper(array, start, end):
    if start >= end:  # 因为 array长度没有变化, 只是内部元素位置变动,用 len()无意
        return
    
    pivot, left, right = start, start + 1, end  # pivot is index of pivot

    while left <= right:
        if array[left] > array[pivot] and array[right] < array[pivot]:
            array[left], array[right] = array[right], array[left]
            left += 1
            right -= 1
            continue
        if array[left] <= array[pivot]:
            left += 1
        if array[right] >= array[pivot]:
            right -= 1
    
    array[right], array[pivot] = array[pivot], array[right]
    
    quick_sort_helper(array, start, right - 1)
    quick_sort_helper(array, right + 1, end)

my_arr = [5, 3, 2 , 8, 12, 9, 10]
quick_sort(my_arr)

```