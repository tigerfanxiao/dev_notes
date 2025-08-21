condition
python代表 false 的值
```python
None
False
0
0.0
''
()
[]
{}
```
具体在if中的用法
```python
if [] # 这里是 [] 表示 False, 任何非空序列是 True
# 这里 max([]) 里面不能放空序列. 用 if word 规避了 max([]) 的计算, 直接返回0
return  max(len(word) for in words) if word else 0
```
python 中的对象, 也可以判断True False
注意: 如果一个对象没有`__nonzero__()`和 `__len__()`两种方法, 则这个对象永远为真

```python
obj.__nonzero__() # 如果设置了这个方法, 且方法的返回值为True 则非空
# 如果没有定义 __nonzero__() 方法
obj.__len__() # 为空, 则对象为空
```

三元表达式
表达式中只能放返回值, 不能是某种执行的动作
```python

value_true if a == a else value_false
```
recursion
```python
def factorial(n):
    return 1 if n == 0 else n * factorial(n-1) 
```


number
```python
positive_infinity = float('inf')
print('Positive Infinity: ', positive_infinity)
```
uuid
```python
# uuid1 一般是host的mac
# uuid4 一般是随机字符串
# fields 字段很多 (time_low, time_mid, time_hi_version, clock_seq_hi_variant, clock_seq_low, node)
uuid.uuid4().fields[-1][:6] # 取最后一个node的前6位
```
### string method

```python
def generate_password(n):
    password = ''.join(
        random.choices(
            string.ascii_uppercase # 所有大写字母
            + string.ascii_lowercase # 所有小写字母
            + string.ascii_letters # 所有字母
            + string.digits, # 所有数字
            + string.punctuation # 所有标点
            k=n, # 抽出几个字符
        )
    )
    return password

def is_valid_password(password):
    if len(password) < 8:
        return False
    if not any(c.isdigit() for c in password): # 是否是数字
        return False
    if not any(c.isupper() for c in password): # 是否是大写字母
        return False
    if not any(c.islower() for c in password): # 是否是小写字母
        return False
    return True

# 字符串的拼接
url = (
        f"https://api.openweathermap.org/data/2.5/weather"
        f"?q={city}&appid={self.api_key}"
    )
        
```

全局变量
```python

global container # 先定义, 再赋值
container = Container()

global container
del container # 删除动作时, 也需要先声明

```
# OOB

```python

# static method 不需要实例化, 就能调用的方法
class MyClass:
	@staticmethod
	def my_staticmethod(self):
		pass

# 直接用类名调用
MyClass.my_staticmethod()

# 属性方法, 通过调用属性的方式来定义方法. 可以用 hasattr 查看
# 这样做的目的在于, 如果修改了用户的first_name, email可以动态计算出来, 但是我们还是像属性一定调用
class Employee:
	def __init__(self, first_name, last_name):
		self.first_name = first_name
		self.last_name = last_name
	@property
	def email(self):
		return f'{self.first_name}.{self.last_name}@email.com'

# 可以通过调用属性的方法直接调用
employee = Employ('xiao','fan')
employee.email # xiao.fan@email
hasattr(Employee, 'email') # True

```

构造类, 其中uuid不能修改 setter
```python
import uuid


class Product:
    def __init__(self, product_name, price):
        self._product_id = self._generate_id() # 这属性被保护起来了
        self.product_name = product_name
        self.price = price

    def __repr__(self):
        return f"Product(product_name='{self.product_name}', price={self.price})"
	
    def _generate_id(self):
        return str(uuid.uuid4().fields[-1])[:6]

    @property
    def product_id(self):
        return self._product_id

    @product_id.setter
    def product_id(self, value): # 这里说明product不能变动
        raise AttributeError("product_id cannot be changed")
```
### Dunder method

```python

# 验证是否是函数, 返回True or False
calllable(Employee.method)

repr
__repr__

```

# Special function

```python 

# 查看对象是否有属性存在
hasattr(obj, 'attr1')
hasattr(my_class, 'attr1')

# 查看属性的类型
getattr(my_class, 'email', None) # 可以查看是否是@property对象
getattr(obj, 'email') # 直接获得对象的属性值

# 查看对象是否属于某一个类
isinstance(obj, int)
isinstance(obj, (int, float))

# all 
if not isinstance(scores, list):
    raise TypeError("Input must be a list")
if not scores:
    raise ValueError("List cannot be empty")
if not all(isinstance(score, (int, float)) for score in scores):
	raise TypeError("Elements of list must be numbers")
if not all(0 <= score <= 100 for score in scores):
	raise ValueError("Elements of list must be between 0 and 100")

# 删除对象
del obj

```

### all, any, filter, map
```python
# return True or False, 常用语条件判断
any(char.isdigit() for char in password)
# 返回boolena值, 所有都是字符
all(char in string.ascii_letters for char in password) 


# 返回的iterable, 需要转换为序列
list(filter(lambda x: x % 2 ==0, my_list))
# map function to object
list(map(len, my_list)
# reduce
from functools import reduce
import operator
reduce(operator.mul, range(1, n+1), 1) # 这个乘法写得好
reduce(lambda x, y: x*y, range(1, n+1), 1)

# 计算一个序列中 True的数量
sum([True, False, True]) == 2 
```

Counter
```python


fruits = ["apple", "banana", "apple", "orange", "banana", "apple"]

Counter(fruits) # 返回一个字典, 里面是每个元素出现的次数
Counter({'apple': 3, 'banana': 2, 'orange': 1})


from collections import Counter

def is_distinct(items):
	counts = Counter(items)
	return all(count == 1 for count in counts.values())
```


# Handle Exception

Input Validation and Propagate Exception
```python
# 如果输入是空序列
if not my_list:
	return None

# 使用instanceof 判断input参数类型和正负值
# isinstance(word, str) # 判断是否是字符串
if not isinstance(width, (int, float)) or width < 0:
    raise ValueError("Width must be a positive number")
```

# Context Manager

Customized context manager 的构造方法
```python
#!/usr/bin/env python3  
# -*- coding=utf-8 -*-  
  
import time  
class Timer:  
    def __enter__(self):  
        self.start = time.perf_counter()  
        print("Starting timer.....")  
        # Optional, return 的对象作为 with 的对象可以使用.  
        # 注意这里能返回的对象只有是在__enter__中定义的, 并操作的  
        # 在__exit__中的定义的和修改的是在 Context Manager 执行完毕退出后才执行的  
        return self  
  
    def __exit__(self, exc_type, exc_val, exc_tb):  
        # 这里打印的是如果 content manager wrap 的代码发生错误, 做 exception 处理的类型, 对象, traceback 信息  
        print(exc_type, exc_val, exc_tb)  
        self.end = time.perf_counter()  
        self.elapsed = self.end - self.start  
        print(f"Elapsed time: {self.elapsed:.6f}")  
  
if __name__ == '__main__':  
  
    # Exception 不处理的话, 会退出程序  
    with Timer() as t:  
        print(1 / 0)  
  
    # 没有 Exception, 但是因为上面代码出现了异常, 就没有执行  
    with Timer() as t:  
        print(t.start) #  
        total = sum(range(1_000_000))  
        print(f"Total time: {total}")
```

context manager 的错误处理
```python
#!/usr/bin/env python3  
# -*- coding=utf-8 -*-  
  
import time  
class Timer:  
    def __enter__(self):  
        """这个例子说明了 customized context manager 要怎么写"""  
        self.start = time.perf_counter()  
        print("Starting timer.....")  
        # Optional, return 的对象作为 with 的对象可以使用.  
        # 注意这里能返回的对象只有是在__enter__中定义的, 并操作的  
        # 在__exit__中的定义的和修改的是在 Context Manager 执行完毕退出后才执行的  
        return self  
  
  
    def __exit__(self, exc_type, exc_val, exc_tb):  
        # 这里打印的是如果 content manager wrap 的代码发生错误, 做 exception 处理的类型, 对象, traceback 信息. 正常运行情况下都是 None
        print(exc_type, exc_val, exc_tb)  
        self.end = time.perf_counter()  
        self.elapsed = self.end - self.start  
        print(f"Elapsed time: {self.elapsed:.6f}")  
  
if __name__ == '__main__':  
  
    # Exception 不处理的话, 会退出程序  
    with Timer() as t:  
        print(1 / 0)  
  
    # 没有 Exception, 但是因为上面代码出现了异常, 就没有执行  
    with Timer() as t:  
        print(t.start) #  
        total = sum(range(1_000_000))  
        print(f"Total time: {total}")
```

使用contextlib定义contextmanager
```python
import unittest
from contextlib import contextmanager

@contextmanager
def temp_file():
	file = open('test.txt', 'w')
	yield file
	file.close()
	os.remove('test.txt')

class MyTestCase(unittest.TestCase):
	def test_file_creation(self):
		# 调用 context manager
		with temp_file() as file:
			file.write('hello, world')
```

# Pycharm
删除一行
ctrl + y

复制一行
ctrl + d

移动一行
ctrl + shift + arrow

vertical Select
alt + shift + insert 退出的化，再操作一次

快速修复
alt + Enter

选中单词
ctrl + w

# windows
alt + F12 打开 terminal
esc 回到代码编辑窗口


# mac
### 代码编辑
option + up 选中整个单词
ctrl + G 选中重复出现的单词, 每按一次 G 会向下查找
option + delete 删除一个单词
cmd + delete 删除完整一行

### 窗口控制
ctrl + tab 在打开的标签页中切换


### 代码跳转
cmd + b 跳转到变量定义的地方


# Terminal

在 Terminal 下面运行 python 解释器是在虚拟环境里

### 可以在虚拟环境中加载某一个 .py文件

```shell
python -i  module.py
```


### 把目录引入 sys.path

![[Pasted image 20240526230710.png]]

### python 脚本模板
只有在 Linux 下生效

- 指定解释器
- 指定编码

```python
#!/usr/bin/env python3
# -*- coding=utf-8 -*-

# 指定 python3 为解释器, 可以直接执行, 需要给文件执行权限 chmod +x file.py
# 在目录下 ./file.py 直接运行. 
# 可以用 python3 file.py 执行, 手动指定解释起
# 保证中文不出现乱码

if __name__ == '__main__':
    pass
```

![[Pasted image 20240526233255.png]]


在 pycharm 中 module 和 package 可以互转

