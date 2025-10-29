内容： 保证一个类只有一个实例，并提供一个访问它的全局访问点
本质上要把构造函数私有化， 不让别人使用



优点： 
* 对唯一实例的受控访问
* 单例相当于全局变量，但防止命名空间被污染
缺点：
* 不能在多线程应用中使用
* 使用object pool pattern

使用场景
* 日志系统，因为日志对象需要操作文件，如果有多个实例在操作一个文件，容易发生冲突
* 数据库的连接。因为数据库的连接池资源有效，一个应用最好只开一个数据库连接实例
* 读取配置文件。如果app全局只有一个配置文件，应该保证只有一个实例


```python

class Singleton:
	# __new__ 会在 __init__前运行
	def __new__(cls, *args, **kwargs):
		if not hasattr(cls, "_instance"):
			cls._instance = super(Singleton, cls).__new__(cls)
		return cls._instance

# 其他自定义的类只需要集成Singleton类，就能实现单例模式

class MyClass(metaclass=Singleton):
	def __init__(self):
		pass 

my_instance = MyClass() # 每一次初始化都是同一个对象
```


singleton的另一种写法
```python
class Singleton(type):
	_instances = {}
	def __call__(cls, *args, **kwargs):
		if cls not in cls._instances:
			cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
		return cls._instances[cls]


class Logger(metaclass=Singleton):
	def log(self, msg):
		print(msg)

```