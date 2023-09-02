
### format

固定长度字符串
```python
'| {:^10} | {:^10} |'.format(col1, col2)
```


两个字符串拼接
```python
my_string = "123" "/223" # = 123223
print(my_string)
```


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
list(string.ascii_uppercase)  # 大写字母
list(string.ascii_lowercase)  # 小写字母
```