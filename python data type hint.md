

只是检查字典类型

```python
from typing import TypedDict


class Interface(TypedDict):  
    name: str  
    ip: str


assert Interface(name='int1', ip='192.168.1.1') == dict(name='int1', ip='192.168.1.1')

```