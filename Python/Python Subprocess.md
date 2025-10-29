

```python

import subprocess


# 执行结果会直接打印在 console 中
# 执行结果会直接打印到 console
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

subprocess.run('ls -al', shell=True, capture_output=True)

# 默认情况下返回的是 byte, 如果要转换为字符串, 可以用 decode
print(p1.stdout.decode()) 

# 转换为字符串, 也可以用text参数
subprocess.run('ls -al', shell=True, capture_output=True, text=True)

# 标准输出
p1 = subprocess.run(['ls', '-la'], stdout=subprocess.PIPE, text=True)

# 重定向到文件
with open('output.txt', 'w') as f:
	p1 = subprocess.run(['ls', '-la'], stdout=f, text=True)

# 错误重定向

p1 = subprocess.run(['ls', '-la', 'dne'], capture_output=True, text=True)
print(p1.returncode) # 如果是错误的返回, returncode 为 1
print(p1.stderr) 

# check=True的时候, 错我信息会在console中打印出来
p1 = subprocess.run(['ls', '-la', 'dne'], capture_output=True, text=True, check=True)


# 错误输出到/dev/null
p1 = subprocess.run(['ls', '-la', 'dne'], stderr=subprocess.DEVNULL)

# 程序关联
p1 = subprocess.run(['cat', 'test.txt'], capture_output=True, text=True)
p2 = subprocess.run(['grep', '-n', 'text'], capture_output=True, input=p1.stdout)
print(p2.stdout)

```