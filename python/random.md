
# choices
从一组数中挑选出指定个数, 返回值是一个list

```python
import random
import string

def generate_id(length=8):
	return ''.join(random.choices(string.ascii_uppercase), length)
```

### shuffle
shuffle是直接修改原始数组的
```python
import random

arr = [1, 2, 3]
random.shuffle(arr)

print(arr)  # 3， 1， 2
```