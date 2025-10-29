```python
# 单层 copy
L2 = [1, 2].copy()

```


建议使用深层 copy
```python
# 深层拷贝
import copy
copy.deepcopy([1, 2, [3, 4]])

# copy后可以通过 id() 方法来查看是不是同一个对象
```