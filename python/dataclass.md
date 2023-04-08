

# field

构建一个空列表初值

```python
from dataclass import dataclass, field

@dataclass
class Email:
	receiver_list: list[str] = field(default_factory=list)
```

