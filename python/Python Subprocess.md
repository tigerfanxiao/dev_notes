

```python

import subprocess

# 执行结果会直接打印在 console 中
subprocess.run('ls') # 只能跑一个命令还不能加参数

subprocess.run(['ls', '-al']) # 命令+参数要放在一个列表中

subprocess.run('ls -al', shell=True) # 命令和参数可以放在一行了

# 如果要获得返回值, 需要增加 capture_output 参数, 此时命令行的结果不会打印在 console 中
p1 = subprocess.run('ls -al', shell=True, capture_output=True)
print(p1.stdout) # stdout 输出byte格式, 不是string
print(p1.stderr)

# 转化 byte 到 string
print(p1.stdout.decode()) # 把 byte 格式, 转化为 string
# 增加 text 参数
p1 = subprocess.run('ls -al', shell=True, capture_output=True, text=True)

```