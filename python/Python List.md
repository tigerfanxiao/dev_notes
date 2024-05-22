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