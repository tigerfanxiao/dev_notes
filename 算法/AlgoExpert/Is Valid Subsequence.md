
# 题目
判断是以序列是不是另一个序列的子集, 且子集的元素顺序和母集保持一致.

# 思考
1. 因为两个集合的元素顺序保持一致, 所以可以用 python 自带的 in 运算符.  因为in 运算符会从头到尾把母集扫描一次. 
2. 因为母集元素更多, 所以从母集中取出元素与子集对比. 只要能匹配到的元素和子集本身的长度一致. 则说明子集是母集的子集.
3. 这个算法的特点在于两个序列一起遍历

### 解法一
```python
# time: O(N) space: O(N)

def is_valid_subsequence(array, sequence):
    
	arr_idx = 0
	seq_idx = 0
	while arrIdx < len(array) and seqIdx < len(sequence):
		if sequence[seq_idx] == array[arr_idx]:
			seq_idx += 1
		arr_idx += 1
	
	return seq_idx == len(sequence)

subsequence = [1, 6, -1, 10]
array = [5, 1, 22, 25, 6, -1, 8, 10]
is_valid_subsequence(array, subsequence)

```

### 解法二

```python

```