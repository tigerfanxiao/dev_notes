# `dataclasses`
### `field(default_factory)`
初始化一个空的列表
```python
from dataclasses import dataclass, field

@dataclass
class Email:
	receiver_list: list[str] = field(default_factory=list)  # 初始化空序列

```
`__post_init__` 这个方法只能用在dataclass包装的类里面
```python
from dataclasses import dataclass, field

@dataclass
class Email:
	def __post_init__(self):  # 在初始化后执行
		print("A")
```
# Pydantic
pydantic 是一个外部包, 需要安装
### Typing List & Tuple
```python
from typing import List

# 字符串序列
def my_func(x:List[str]):
	pass
	
# 整数序列
List[int]

my_tuple:tuple[str, ...] = ('one', 'two') 
```

### Typing Dictionary
```python
from typing import TypedDict

class Interface(TypedDict):  
    name: str  
    ip: str

assert Interface(name='int1', ip='192.168.1.1') == dict(name='int1', ip='192.168.1.1') # True
```

可有可无的参数
```python
from typing import Optional

# specific Type and default value is None
def my_func(a: Opitional[int] = None):
	pass

```

```python
from typing import Optional

def greet(name: Optional[str]) -> str:
    if name is None:
        return "Hello, Guest!"
    return f"Hello, {name}!"
```
# Check Typing with `mypy`

```shell
# 安装mypy
pip install mypy

# 运行 mypy 来检查py文件的typing
mypy my_file.py
```

```python
def foo(a: int, b: int) -> int:  
    return a + b  
  
  
if __name__ == "__main__":  
    foo("a", 1)  # pycharm 在这里会提示 a 不是int
```

