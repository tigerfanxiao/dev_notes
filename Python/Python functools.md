
# reduce

```python
from functools import reduce

# 16 把一个序列累加和
list_a = [1, 2, 3, 4]
reduce(lambda i, j: i + j, list_a, 10)  # 这里10是初始值

```
