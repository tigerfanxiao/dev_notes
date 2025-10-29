

### print instance
```python

class Person():
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
	def __str__(self):
			return f"name: {self.name} color: {self.color}"
```

### 查找和设置对象属性
### getattr setattr

```python

attribute = getattr(object, "attribute_name")

# change the attribute 
setattr(object, "attribute_name", value)
```


###  获得所有属性
vars
```python
from dataclasses import dataclass

@dataclass
class Fruit:
	name: str
	color: str


apple = Fruit(name='apple', color='red')
vars(apple)  # 转为一个dict， 已经包含了key和value
apple.__dict__  # 返回 key值， 不包括dunnder属性

Fruit.__annotations__.keys() # 获取类的属性值


for attr, value in apple.__dict__.items():
        print(attr, value)


```


### anotation
可以规避下面这种错误， 就是再用B的时候， B还没有定义

```python
from __future__ import annotations  # 必须放在首行

class C:
    @classmethod
    def from_string(cls, source: str) -> C:
        ...

    def validate_b(self, obj: B) -> bool:
        ...

class B:
	..
```