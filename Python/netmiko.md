是一个支持 ssh 的库

### 建立连接

```python
import netmiko 

# 初始话连接
device = ConnectHandler(
			host="csr1kv1", 
			username="cisco", 
			password="cisco", 
			device_type="cisco_xe"
		)
device.session_timeout = 180 # 可以手工配置, 如果 180 面没有操作, 就自动断开
device.is_alive() # 连接是否是活的
device.disconnect()  # 断开连接
device.establish_connection() # 建立连接

```

### 发送命令

```python
# 发送字符串
device.send_command("show version")
# 把文本配置发送出去
device.send_config_from_file("/home/student/config_files/interface_conf.cfg")

# 发送一组命令, 每一行命令为一个元素
interface_config = ['interface GigabitEthernet1', 'description HR']
device.send_config_set(interface_config)

# 保存交互过程中的日志
device.open_session_log("/home/student/logs/CSR.log")

```