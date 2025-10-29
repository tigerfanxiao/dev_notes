
### 环境变量

```shell
pip install python-dotenv
```

`.env` 可以放在调用 `load_dotenv` 的目录上级
```shell
FLASK_ENV=development
DEBUG=True
TESTING=True
SECRET_KEY=xiao1234
DB_USERNAME=postgres
DB_PASSWORD=postgres
DB_HOST=127.0.0.1
DB_NAME=fkcommerce
```

```python
from dotenv import load_dotenv
load_dotenv() # 读取.env文件的环境变量

my_variable = os.getenv("SECRET_KEY")
```
# 路径

```python
# constant.py
import os

os.getcwd() # 获得当前路径

os.chdir('/root') # 切换目录 

# 把config.ini位置固定在constant同级
CONFIG_FILE_PATH = os.path.join(os.path.dirname(__file__), 'config.ini')

# 查看是否是文件
os.path.isfile(CONFIG_FILE_PATH)

# 查看是否是文件夹
os.path.isdir(DIR)

# 查看文件夹是否存在
os.path.exists(r'c:\users\admin')

# 查看文件大小
os.path.getsize(r'g:\win10sn.txt')

# 查看文件的绝对路径
os.path.abspath(CONFIG_FILE_PATH)

# 如果文件是软链接，也可以得到正真的文件路径
os.path.realpath(PATH)

# 查询父路径
os.path.dirname(__file__)

# 提取文件的绝对路径 
os.path.dirname(os.path.abspath(__file__))
os.sep # 路径分隔符 
os.linesep # 换行符
os.path.abspath(os.path.curdir)

os.path.split(my_path) # 拆分文件夹和文件
os.path.splitext(my_path) # 把文件扩展名分割出来
os.path.join(dir, file) # 拼接文件名


# 打印路径中的文件名
os.path.basename(path)

# 创建文件夹
os.mkdir(path)

# 列出目录下的所有文件和子目录
os.listdir(path) 

# 删除文件
os.remove(filename)

# 查看环境变量
# 应用场景, 如果要改变容器的行为,在拉起容器的时候, 设置一些环境变量
# 通过下面的代码就可以读取相应的环境变量. 然后将这些环境变量放到容器中
os.environ.get("PYTHONPATH")

os.getpid() # 查看进程id
os.kill(26923, 1) # 杀进程 26923 
```

# 读取文件夹列表

读取一层

```python
import os

# 返回但是dir对象， 里面有name属性
for dir_path in os.scandir(ATTACHMENT_FOLDER):
	dir_path.name  # 变量所有文件夹
```


### 读取所有文件， 但是不包含文件夹
```python


files = (file for file in os.listdir(path) 
         if os.path.isfile(os.path.join(path, file)))


# 生成器
def list_only_files(path):
    for file in os.listdir(path):
        if os.path.isfile(os.path.join(path, file)):
            yield file

```

# 命令行
- `os.system()`
- `os.popen().read()`


运行命令行
```python
import os

# 只是打印结果, 没有返回值
os.system('ipconfig')

# 可以获得返回值, 自动化中常用
# 如果命令执行错误, 则没有返回
os.popen("ifconfig en0").read()
```

如果要获得错误执行的信息

```python

import subprocess
import io

def system_cmd(cmd):
	proc = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, bufsize=-1)
	proc.wait()
	stream_stdout = io.TextIOWrapper(proc.stdout)
	stream_stderr = io.TextIOWrapper(proc.stderr)

	str_stdout = str(stream_stdout.read())
	str_steerr = str(stream_stderr.read())

	return str_stdout, str_stderr

```