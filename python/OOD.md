

### getattr setattr

```python

attribute = getattr(object, "attribute_name")

# change the attribute 
setattr(object, "attribute_name", value)
```


### vars
```python
from dataclasses import dataclass

@dataclass
class Fruit:
	name: str
	color: str

apple = Fruit(name='apple', color='red')
vars(apple)  # 转为一个dict， 已经包含了key和value
apple.__dict__  # 返回 key值， 不包括dunnder属性

for attr, value in apple.__dict__.items():
        print(attr, value)


```