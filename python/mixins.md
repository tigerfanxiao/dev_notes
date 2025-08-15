A mixins is a class with the single purpose to add funtionality to other class
主要用来解决层级很深的继承问题

```python
from dataclasses import dataclass

@dataclass
class Shape:
	x: int
	y: int

class Serializer: # this is the mixin
	def serialize(self):
		print(",".join([f"{k}={v}" for k, v in self.__dict__.items()]))


class Rectangle(Shape, Serializer):
	def __init__(self, x, y, width, height):
		super().__init__(x, y)
		self.width = width
		self.height = height

class Circle(Shape, Serializer):
	def __init__(self, x, y, radius):
		super().__init__(x, y)
		self.radius = radius

```