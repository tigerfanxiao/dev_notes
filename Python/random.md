
```python
random.randint(1, 255) # 随机整数
```

## Choices VS Sample

- Sample 取值不会重复
- Choices 取值可以重复
### choices
从一组数中挑选出指定个数, 返回值是一个list

```python
import random
import string

def generate_id(length=8):
	 # 可以有重复值 ORTERPTQ
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


### shuffle
shuffle是直接对原数组进行操作
```python
import random

arr = [1, 2, 3]
random.shuffle(arr)

print(arr)  # 3， 1， 2 原数组已经被修改
```