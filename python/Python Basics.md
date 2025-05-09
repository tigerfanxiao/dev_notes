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