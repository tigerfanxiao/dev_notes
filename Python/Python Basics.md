# Python Data Structure
## `int`
在金融系统里一般是用int, 因为int可以存很大的数字, 内存有多大就存多大. 但是精度不高, 计算慢. 
float 精度高, 适合需要实时计算的物理系统
在金融系统中, 100元, 表示为10000 分来存储. 这样可以存储很大的数据
## `list`
适用一序列的对象, 但是不需要查找
```shell
my_list.append(2) # append a value into list
[1, 12, 3, 12].count(12) # count the number of occurence of 12
my_list.clear() # clear the content of list
del my_list[3] # remote the item at index 3
my_list.index(5) # return the index of first occurence of 5 
[1, 2] + [3, 4, 5] # concatenate two lists
```
### copy of List
```python
my_list = [1, 2, 3]
my_list_copy = my_list[:] # modify copy list won't impact original list
```
## `string`
### f-string
```shell
# print 2digit decimal string
amount: int = 3000
formatted_str = f"The amount is ${amount / 100:.2f}"
```

### `enum` & `auto`

```python
from enum import Enum, auto
# auto() will assign an integer to the variable
class Month(Enum):
	JANUARY = auto()
	FEBUARY = auto()
	MARCH = auto()
	APRIL = auto()
	MAY = auto()
	JUNE = auto()
	JULY = auto()
	AUGUST = auto()
	SEPTEMBER = auto()
	OCTUBER = auto()
	NOVENBER = auto()
	DECEMBER = auto()
	
print(Month.JANUARY)
```

## `tuple`
适用于一堆不同类型对象, 但是又不想要构建class类
### use `tuple` to get multiple return 

```python
def birthday_month_year() -> tuple(Month, int):
	return Month.JUNE, 1988
```
## `set`
如果要存储去重的对象
## `dictionary`
```python
# 删除字典
del my_dict
# clear key value inside of dic
my_dict.clear()
# combine two dict and generate a new
my_dict | {"key": "value"}

```

### `Lambda` for Sorting

一般用 sorted 函数加 lambda 来做排序

```python
# 对子序列里的第二个位置数字排序
sorted([[2, 2], [1, 3]], key=lambda x: x[1]) 

# 对子序列里的第一个位置排序, 然后对第二个位置排序
sorted([[2, 2], [1, 3]], key=lambda x:(x[0], x[1]))
```


对 ip 地址进行排序
```python
import ipaddress # 内置库

ip_list = [
		   '172.16.12.123',
		   '192.168.1.123',
]

sorted(ip_list, key=lambda ip: ipaddress.ip_address(ip))
```

### `argparse`
```python
from argparse import ArgumentParser

def gyt_argparse(host, filename, iface):
	print(host)
	print(filename)
	print(iface)

usage = "usage: python arg_parse.py -t host -f filename -i interface"

parser = ArgumentParser(usage=usage)
# 基于参数的
# 一个短参数, 一个常参数
parser.add_argument("-f", "--file", dest="filename", help="Write content to FILE", dafault="1.txt", type=str)

parser.add_argument("-i", "--interface", dest="iface", help="Specify an interface" dafault=1, type=int)
parser.add_argument("-h", "--host", dest="host", help="Specify an host", dafault="10.1.1.1",type=str )


# 基于位置的, 至多有一个
parser.add_argument(nargs='?' dest="host", help="Specify an host", dafault="10.1.1.1",type=str )
# 基于多个位置
parser.add_argument(nargs='*' dest="host", help="Specify an host", dafault="10.1.1.1",type=str )

args = parser.parse_args()			
qyt_argparse(args.hosts, args.filename, args.iface)
```

## class
class 根据用户可以分为data oriented 和 function oriented. 我们用dataclass来定义那种属性字段很多的class. 同时使用dataclass来定义的类, 在打印对象时, 可以把所有属性都打印出来
```python



@dataclass(frozen=True) # the instance cannot be modified after modification
class Vehicle:
    brand: str
    model: str
    color: str
    license_plate: str
    fuel_type: FuelType = FuelType.ELECTRIC
    # default_factory can be function
    accessories: list[Accessory] = field(default_factory=lambda: [Accessory.AIRCO])
```

## function
### pure function & side-effect function
pure function 函数只是操作输入的参数, 不会去操作函数体意外的变量
side-effect function 函数会操作函数体意外的变量, 因为不确定函数的影响范围. 造成排错困难
建议尽量写pure function, 或者对side-effect function 引入变量, 把它变成pure function

function 本质上也是一个类, 是一个写了 `__call__`方法的类
```python
(3).__class__ # 打印对象的所有属性
```
# Python Typing
Python是dynamic type 和 Strong type
- Dynamic type表示, 变量本身不会限制被赋值的类型, 一个变量可以被赋值为字符串, 然后再被赋值成整数
- Strong type 表示, 整个整数+一个字符串在python中是不允许的. 但是弱类型语言JS中是允许的

函数
```python
from typing import Callable

# first argument is argument of function, the second argument is output
int_function = Callable[[int], int]

def add_three(x:int) -> int:
	return x +3

def main():
	my_var: int_function = add_three
	print(my_var(5))

if __name__ == "__main__":
	my_var()
```

