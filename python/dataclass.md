

# field

构建一个空列表初值

`__post_init__` 这个方法只能用在dataclass包装的类里面

```python
from dataclasses import dataclass, field

@dataclass
class Email:
	receiver_list: list[str] = field(default_factory=list)  # 初始化空序列

	def __post_init__(self):  # 在初始化后执行
		print("A")
	
```

