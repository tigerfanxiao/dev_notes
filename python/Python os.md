
# 路径相关的

```python
# constant.py
import os

os.getcwd() # 获得当前路径

# 把config.ini位置固定在constant同级
CONFIG_FILE_PATH = os.path.join(os.path.dirname(__file__), 'config.ini')

# 查看文件是否存在
os.path.isfile(CONFIG_FILE_PATH)

# 查看文件夹是否存在
os.path.isdir(DIR)

# 查看文件的绝对路径
os.path.abspath(CONFIG_FILE_PATH)

# 如果文件时软链接，也可以得到正真的文件路径
os.path.realpath(PATH)

# 查询父路径
os.path.dirname(__file__)

# 打印路径中的文件名
os.path.basename(path)

# 创建文件夹
os.mkdir(path)

# 列出目录下的所有文件和子目录
os.listdir(path) 

# 删除文件
os.remove(filename)
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

```python
import os
os.popen("ifconfig en0").read()
```