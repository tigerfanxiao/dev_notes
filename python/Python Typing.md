# Pydantic

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

### Optional

Optional for function
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
pip install mypy

# in terminal run
mypy my_file.py
```

```python
def foo(a: int, b: int) -> int:  
    return a + b  
  
  
if __name__ == "__main__":  
    foo("a", 1)  # pycharm 在这里会提示 a 不是int
```

