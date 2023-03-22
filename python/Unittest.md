

```python
import unittest

# 需要继承一个TestCase类
class TestFunction(unittest.TestCase):
	def setUp(self) -> None:
		# run every time before test function run
		pass
		
	def tes_func(self) -> None:
		self.assertEqual(1, 1)
```