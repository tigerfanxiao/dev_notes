
### 两个实例创建方法
- `ipaddress.ip_network()` 构建
- `ipaddress.ip_address()` 构建 `IPv4Address`类

```python
import ipaddress

network = ipaddress.ip_network('192.168.1.0/24')
# 网段对象

for ip in network:
	print(ip)  # 打印出网段中的所有 IP

# ip address 对象
type(ipaddress.ip_address('192.168.1.1'))
# ipaddress.IPv4Address 类

# 判断IP是不是在这个网段中
ipaddress.ip_address('192.168.1.1') in network

```