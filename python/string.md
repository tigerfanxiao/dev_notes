
### join()
join默认是放序列的，但是也可以放 comprehesion
```python
def get_random_string(length)
	letters = string.ascii_lowercase
	# 重复取length次
	return ''.join(ranndom.choice(letters) for i in range(length))
```


字母表 Alphabet

```python
import string
list(string.ascii_uppercase)
```