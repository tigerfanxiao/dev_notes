# 问题
1. 怎么在vscode中集成mypy
2. 怎么手动运行mypy

### 序列

```python
from typing import List

# 字符串序列
List[str]
# 整数序列
List[int]
```


# 检查 Mypy
mypy是一个可以检查python type的包

```python
def foo(a: int, b: int) -> int:  
    return a + b  
  
  
if __name__ == "__main__":  
    foo("a", 1)  # pycharm 在这里会提示 a 不是int
```