

```python
from pathlib import Path
# 创建一个Path对象
path = Path('./example.txt')
# 查看文件是否存在
path.is_file() 
# 打印路径
path.resolve()
# 父路径
path.parent

# 支持 forward slash / 操作符
(Path(__file__).parent / "../data").resolve() # resolve 打印绝对路径
```