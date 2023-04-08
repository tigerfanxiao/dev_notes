
# reduce

```python
# 检查一个序列一定时递增的
bool(lambda test_list: reduce(lambda i, j: j if i < j else 9999, test_list) != 9999)

# 这里 reduce(lambda i, j: j if i < j else 9999, test_list) 的结果时序列最后一个元素或者是9999
```
