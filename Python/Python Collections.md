
### `defaultdict`
带有初值的字典。 保证初始化的时候，特别是空列表，这种行为不会变动

```python
from collection import defaultdict

subscribers = defaultdict(list)

```

### Counter
两个 Counter 对象可以直接比较
```python
from collections import Counter

def anagrams(str1: str, str2: str) -> bool:
    if len(str1) != len(str2):
        return False
    return Counter(str1) == Counter(str2)


```