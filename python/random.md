
### choices
从一组数中挑选出指定个数, 返回值是一个list

```python
import random
import string

def generate_id(length=8):
	 # 可以由重复值 ORTERPTQ
	return ''.join(random.choices(string.ascii_uppercase, k=length))
	
```

### sample
sample 不会有重复的取值
```python
import string
import random

def get_sample(length=8)
	# 'NUBRXKES' 取值不重复
	return ''.join(random.sample(string.ascii_uppercase, length))
```

评：sample和choices的区别在于sample取值不会重复

### shuffle
shuffle是直接修改原始数组的
```python
import random

arr = [1, 2, 3]
random.shuffle(arr)

print(arr)  # 3， 1， 2
```