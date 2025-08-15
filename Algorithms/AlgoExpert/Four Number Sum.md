# 问题描述
在一个序列中找到 4 个数字累加的和是 target_sum

# 思考
1. 分而治之的方法, 将 4 个数字的和, 分为两组, 即两组数字的和为 target sum
2. 使用 hashtable 来保存计算过的一组数字的和, 当我们获得一组新的数字时, 查询 hash table 看是不是两组数字的和为 target sum
3. 保存 hashtable 时有几个问题
	1. 一个和存在多种组合, 所以 hashtable 的结构是 `{current_sum: [[a, b], [c, d]]}`
	2. 如何保证两组数字的加法顺序一致. 只把当前元素左侧的元素存入 hash 表

```shell
# 下面三组数字的和是相同的. 如果我们这三组数字左边的和和右边的和都存到 Hash table, 则会出现重复的答案. 
(a + b) + (c + d)
(a + c) + (b + d)
(a + d) + (b + d)

# 我们只需要 (a + b) + (c + d) 一种顺序
# 我们假设 c 是当前计算元素
# 我们先check hash 表里有没有 a + b 使得 a + b + c + d = target_sum
# 如果有, 则是想要的结果
# 如果没有, 则把 a + c, b + c 存到 hash 表中, 为后面 d 作为计算元素时使用
```

代码 
1. 为什么左右两个序列都会用到 array[i]? 其实这两个元素是不一样的. all_pair_sums 中保存的和, 一定是上一轮循环中的 i 和左边的元素的和. 而不会是当前 i

```python

# Avg Time: O(n^2) Max Time: O(n^3)
# space: O(n^2)
def four_sum(array, target_sum):
    all_pair_sums = {}
    quadruplets = []
    # 第一个元素没有左侧元素, 所以此时all_pair_sum为空, 不需要参与迭代
    # 最后一个元素因为右侧元素为空, 也不需要参与迭代
    for i in range(1, len(array) - 1):
        for j in range(i + 1, len(array)):
            current_sum = array[i] + array[j]
            diff = target_sum - current_sum
            if diff in all_pair_sums:
                for pair in all_pair_sums[diff]:
                    quadruplets.append(pair +[array[i], array[j]])
            # 右侧元素和与 target_sum 的差, 如果不在 hash 表中, 则什么都不做
        # 只把左侧的元素的和存入 hash 表
        for k in range(i):
            current_sum= array[k] + array[i]
            if current_sum not in all_pair_sums:
                all_pair_sums[current_sum] = [[array[k], array[i]]]
            else:
                all_pair_sums[current_sum].append([array[k], array[i]])
    return quadruplets

test_array_four_sum = [7, 6, 4, -1, 1, 2]
four_sum(test_array_four_sum, 16)
```