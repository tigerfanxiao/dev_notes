
使用场景： 
1. 如果有多种算法来处理相同的问题，且需要根据场景配置不同的算法

写法： 
1. 定义一个通用接口用来约束不同的strategy
2. 定义Context类，把strategy和数据一起打包
	1. Context类中，可以执行strategy
	2. Context类中，可以切换strategy

```python

from abc import ABCMeta, abstractmethod

# 通用接口用来约束Strategy
class Strategy(metaclass=ABCMeta):
	@abstractmethod
	def execute(self, data) # 传入需要处理的数据
		pass

class SlowStrategy(Strategy):
	def execute(self, data):
		print("it is slow strategy")


class FastStrategy(Strategy):
	def execute(self, data):
		print("it is fast strategy")


class Context:
	def __init__(self, strategy, data):
		self.strategy = strategy
		self.data = data

	def set_strategy(self, strategy):
		self.strategy = strategy

	def do_strategy(self):
		self.strategy.execute(data=self.data)


# 调用
# 初始化context
if __name__ == '__main__':
	strategy = SlowStrategy()
	data = 'fanxiao'
	context = Context(strategy, data)
	context.set_strategy(FastStrategy())
	context.do_strategy()
```


