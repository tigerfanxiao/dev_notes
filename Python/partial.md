partial函数的问题
1. 他不能指定替代某一个参数， 他只能按照参数的顺序去填充。 一半来说剩余的参数空余的
2. 不能用在类中的方法上


通过函数生成函数
```python
from functools import partial

# A normal function
def f(a, b, c, x):
	return 1000*a + 100*b + 10*c + x

# A partial function that calls f with
# a as 3, b as 1 and c as 4.
g = partial(f, 3, 1, 4)

# Calling g()
print(g(5))

```