
# 思想
1. 解决子问题, 或者说局部问题. 比如在一个序列中, 
### 解法一
1. 通过元素两两对比, 将最大的元素置换到序列的最后一位
2. 然后在子序列中运用步骤 1
```python
def bubble_sort(arr):
    j = len(arr) - 1
    while j > 0:
        i = 0
        while i < j:
            if arr[i] < arr[i+1]:
                arr[i+1], arr[i] = arr[i], arr[i+1]
            i += 1
        j-= 1
    return arr
```

### 解法二 递归解法
```python
def bubble_sort_recursive(arr):
    def bubble_sort_helper(arr, sub_arr_length):
        if sub_arr_length > 1:
            bubble_sort_helper(arr, sub_arr_length - 1)
        if arr[sub_arr_length - 1] > arr[sub_arr_length]:
            arr[sub_arr_length - 1], arr[sub_arr_length] = arr[sub_arr_length], arr[sub_arr_length - 1]
        return arr
    return bubble_sort_helper(arr, len(arr) - 1)
    
```

解法三 for loop
```python
def bubble_sort_forloop(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j+1], arr[j]
    return arr

bubble_sort_forloop(test_array_1)
```