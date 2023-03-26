
```python
# constant.py
import os

# 把config.ini位置固定在constant同级
CONFIG_FILE_PATH = os.path.join(os.path.dirname(__file__), 'config.ini')

# 查看文件是否存在
os.path.isfile(CONFIG_FILE_PATH)

# 查看文件的绝对路径
os.path.abspath(CONFIG_FILE_PATH)

# 查询父路径
os.path.dirname(__file__)


```