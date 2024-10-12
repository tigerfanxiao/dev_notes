### get method
如果 key 不存在
```python
my_dict = {'name': 'xiao'}
my_dict['age'] # 如果 key 不存在会报错

my_dict.get('age') # 如果 key 不存在, 会返回 None

# if key does not exist, return default value
my_dict.get('age', 'UNKNOWN KEY')
```

### update method
更新字典中的键值对. 如果键已经存在, 则更新值. 如果键不存在, 在添加新的键值对
```python

my_dict = {'name': 'xiao'}
my_dict.update({'name': 'fan'})  # 更新字典中的键值对
```


pop 方法
```python
my_dict = {'name': 'xiao', 'age': 20}
my_dict.pop('age')  # 去掉一个键值对, 并把去掉的值返回
```

### `zip` method

使用zip构建字典
```python
my_dict = zip(key_list, value_list)
```

zip 可以方便打印三个列表
```python

my_list = []
for a, b, c in zip(a_list, b_list, c_list):
	my_list.append({'a': a, 'b': b, 'c': c})

from pprint import pprint
pprint(my_list)  # 自动换行, 缩进


```

### `dict.fromkeys()` 去重

使用 fromkeys方法可以去重, 且保持原来的序列的顺序. 而 set 则无法保证顺序
```python
# 去重并保证顺序
items = ['a', 'b', 'c', 'a']
unique_items = list(dict.fromkeys(my_items))
```