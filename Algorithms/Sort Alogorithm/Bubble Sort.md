
# 思想
1. 假设序列有n个元素， 首先找到整个序列的最大值， 然后把这个元素调换到序列的最末尾。 因为最大值已经在应该在的位置， 下面我们缩小序列范围，查找n-1个元素序列中的最大值。 
2. 序列的长度会在每一次循环中减小一，每一次循环我们都只要把最大值移动到序列的末尾就行
### 解法一 while loop
1. 通过元素两两对比, 将最大的元素置换到序列的最后一位
2. 然后在子序列中运用步骤 1

```python
def bubble_sort(arr):
    j = len(arr) - 1  # j 用来控制子序列的长度， 每次循环都会减少1
    while j > 0:
        i = 0
        while i < j:  # i 用来遍历当前的子序列
            if arr[i] < arr[i+1]:
                arr[i+1], arr[i] = arr[i], arr[i+1]
            i += 1
        j -= 1
    return arr
```

### 解法二 for loop

```python
def bubble_sort_forloop(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j+1], arr[j]
    return arr

bubble_sort_forloop(test_array_1)
```

