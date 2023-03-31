
```python
# constant.py
import os

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

# 创建文件见
os.mkdir(path)


```