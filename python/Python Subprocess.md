

```python

import subprocess

# 执行结果会直接打印出来
subprocess.run('ls') # 只能跑一个命令还不能加参数

subprocess.run(['ls', '-al']) # 命令+参数要放在一个列表中

subprocess.run('ls -al', shell=True) # 命令和参数可以放在一行了
```