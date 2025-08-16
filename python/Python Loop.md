
### 异常退出
```python
import time

while True:
	try: 
		time.sleep()
	except KeyboardInterrupt:
		print("you have stoped the program")
		break
```

### 信号退出

```python

import time
import signal
import sys

def signt_handler(signum, frame): # 定义处理方式
	print("接受到管理员 ctrl + c")
	print("退出程序")
	sys.exit()

# 用 signt_handler 函数来处理这个系统信号
signal.signal(signal.SIGINT, sigint_handler)

while True:
	time.sleep(2)
	print("请输入 Ctrl + C 来停止循环")
```