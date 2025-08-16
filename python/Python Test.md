
# Unit test
```python
import unittest

def add(x, y):
	return x + y

class TestAddition(unittest.TestCase):
	def test_add_positive_numbers(self):
		result = add(2, 3)
		self.assertEqual(result, 5, msg=None)

		# 不等于
		self.assertNotEqual(result, 5, msg=None)
		# 其他assert True or False
		self.assertTrue(result, True)
		self.assertFalse(result, False)

		# check None
		self.assertEqual(resut, None)
		self.assertIsNone(obj, msg=None)
		self.assertIsNotNone(obj, msg=None)

		# less than
		self.assertLess(obj1, obj2)
		self.assertGreater(obj1, obj2)

		# 如果要判断一个属性是否是用@property 定义出来的
		self.assertIsInstance(getattr(class,'email', property))

		# 判断 Exception正常触发
		with self.assertRaises(ValueError):
			rect = Rectangle(-4, 5)
		# 不使用context manager
		self.assertRaises(TypeError, callable, *args, **kwargs)
			
		# 是否在容器中
		self.assertIn(member, container, msg=None)
		self.assertNotIn(member, container, msg=None)

		# 在比较浮点数计算, 查看到小数点后2位
		self.assertAlmostEqual(first_float, second_float, places=2, delta=0.5)
	

if __name__ == '__main__':
	unittest.main()
```

assert warning
```python
import unittest
import warnings

def generate_warning():
	# 注意这里的UserWarning 或者是Warning都是不需要引入包就能用的
	# 因为Warning是Exception的自己
	# import warnings 只是为了调用 warnings.warn() 方法
	warnings.warn("This is a warning", UserWarning)

class TestWarning(unittest.TestCase):
	def test_warning(self):
		with self.assertWarns(UserWarning):
			generate_warning()
```

asset 只是用于开发环境, 不用于生产环境
```python 
countries = ['POL', 'ENG', 'GER', 'USA', 'ITA']
is_italy = 'ITA' in countries

assert is_italy, 'Italy not found in the countries list.'
```
# Fixture
Fixture的作用
1. 创建公共的实例
2. `setUp`和`tearDown` 每个`test_` 方法都要重新用setUp构造一遍, 并用 tearDown拆除一遍
3. `setUpClass()`和`tearDownClass()` 整个TestCase中的所有`test_` 方法都只构造一次, 执行完之后, 拆除
4. `setUpModule()`和 `tearDownModule()` 是整个module都能用
### setup & teardown
```python
class TestCalculator(unittest.TestCase):
	def setUp(self) -> None:
		self.calculator = Calculator() # 整个对象可以给所有的test_ 方法用

	def tearDown(self) -> None:
		del self.calculator # 销毁实例

```
`setupClass` 和 `tearDownClass`
```python
import unittest

class MyTestCase(unittest.TestCase):

	@classmethod
	def setUpClass(cls):
		# One-time setup for the entire test case class
		cls.server = start_test_server()

	@classmethod
	def tearDownClass(cls):
		# One-time teardown for the entire test case class
		stop_test_server(cls.server)

	def test_example1(self):
		# 在引用 setUp中定义的 cls.server 时, 需要引用
		self.server 
		self.assertEqual()
		

	def test_example2(self):
		# Test code that uses the test server
		self.server
		self.assertEqual()
```

`setUpModule`
```python

def setUpModule():
	global calc
	calc = SimpleCalc()
```


使用context manager 也可以实现, 文件的初始化和删除

```python
import unittest
from contextlib import contextmanager

@contextmanager
def temp_file():
	# Create a temporary file
	file = open("temp.txt", "w")
	yield file
	# Clean up the temporary file
	file.close()
	os.remove("temp.txt")

class MyTestCase(unittest.TestCase):
	def test_file_creation(self):
		with temp_file() as file:
			# Test code that uses the temporary file
			file.write("Hello, World!")
```

创建临时文件的包 `tempfile`
```python
import tempfile
# 一般情况下TemporaryFile 创建的文件在close后, 会自动删除. 所以这里设置了delete=False
# 在close之后, 也不会被删除
# NamedTemporaryFile 会自动生成一个文件名, 通过 fp.name 引用
fp = tempfile.NamedTemporaryFile(mode='w+', delete=False)
#
fp.write(b'hello, world') # 这里写入后 seek在文件末尾
fp.close()
# 如果文件刚写完, 还没有被关闭, 则需要读取文件内容的话需要把光标放到文件头部
fp.seek(0) 
# 如果文件已经close, 则可以是直接读取, 不用seek(0)
assert fp.read() == b'hello, world', 'not equal content'

# 删除文件
os.unlink(fp.name) 
os.remove(fp.name) # 两个效果一样
```

### skip test
- 如果是使用TDD的开发模式, 在实现代码开发前, 先写测试代码, 会遇到有些测试需要先跳过, 因为开发还没有完成
- 也可能是因为有些代码是特定系统环境下才需要测试的, 在生产环境中不需要测试, 也可以跳过. 比如手机端的测试代码, 不需要在系统端测试
- 有条件的跳过某些测试用例. 比如代码中自动调取当天的日期, 因为当天每天都变, 
```python
import unittest

class MyTestCase(unittest.TestCase):
	@unittest.skip("Skipped for demonstration purposes")
	def test_skip_example(self):  # Test code that would be skipped
		pass
```
有条件的skip
```python
class TestContainer(unittest.TestCase):
	@unittest.skipIf(date.today().day % 2 != 0, 'Skipping odd days.')
	def test_code_ends_with_0_on_even_days(self):
		c = Container()
		self.assertTrue(
		c.code.endswith('0'), 'Expected code to end with 0.')
```
除非满足条件, 否则就跳过
```python
@unittest.skipUnless()
```

构造一个测试文本文件
```python
class TestFileReader(unittest.TestCase):
	def setUp(self):
		self.filename = 'test.txt'
		with open(self.filename, 'w') as f:
			f.write('Hello, world!')

	def tearDown(self):
		os.remove(self.filename)

	@unittest.skipUnless(os.path.isfile('test.txt'), "File not found")
	def test_read_data(self):
		reader = FileReader(self.filename)
		data = reader.read_data()
		self.assertEqual(data, 'Hello, world!')
```

如果测试代码还没开发完成, 则可以用 `self.fail()` 直接使测试失败. 加上skip装饰器直接跳过
```python
import unittest

class DataTransformer:
	def transform(self, data):
	# Code to transform data
		return transformed_data

class TestDataTransformer(unittest.TestCase):
	@unittest.skip("Test skipped as not implemented yet")
	def test_transform(self):
		# 立刻使这次测试失败
		self.fail("Test not implemented yet")

```
### Unittest.mock
```python
from unittest.mock import Mock, MagicMock, patch
# Create a basic mock object
mock_obj = Mock(name='first_mock')
# attribute
mock_obj.call_count
# Create a MagicMock object that behaves like a real object
magic_mock_obj = MagicMock()
```

you can configure the behavior of mock objects by assigning return values, side effects, or exceptions that they should raise when called. You can also inspect how they were called.
```python
# 被模仿函数的返回值
mock_func.return_value = 42
mock_obj.json.return_value = json_obj # 表示被mock对象的json方法的返回值

# 如果被测试的函数有多个Exception的结果
mock_obj.side_effect = [Exception(1), Exception(2), True]
mock_obj.return_value = True # 被测试函数的最后一个返回值

# 带上测试函数的参数
mock_obj.assert_called_once_with(arg1, arg2)
```
Using Patching for Context Manager: To temporarily replace an object or function with a mock during a specific scope of code, you can use the patch context manager.

```python
from unittest.mock import patch
with patch('module_name.function_name', new=mock_obj):
	# Code that uses the mocked function
```

- **Verifying Calls**: You can use various methods like `assert_called_with`, `assert_called_once_with`, and `assert_not_called` to verify how the mock object was called during the test.
```python
mock_obj.assert_called_with(1, 2) # 被调用的参数
mock_obj.assert_called_once_with(1, 2) # 被调用一次
self.assertEqual(mock_obj.call_count, 3) # 被调用3次
mock_obj.assert_not_called()
```

mock function
```python
import random
from unittest.mock import Mock

techs = ['python', 'sql', 'java', 'aws', 'c++']

random.choice = Mock(return_value='aws')
print(random.choice())
```

在实际工作中, 使用 patch 的情况最多
- `patch` 的路径非常重要. 一般是`<要替换函数所在的 module>.<要被替换的函数>` 
- 如果要替换的函数和测试函数在一个 module 里, 则`<本 module>.<要被替换的函数>`
- 被 `patch`的装饰的函数需要增加一个 mock变量
```python
import random
from unittest.mock import patch

techs = ['python', 'sql', 'java', 'aws', 'c++']

with patch('random.choice', return_value='aws'):
	print(random.choice(techs))
```

Example, use mock to make random choice always return 'python'
```python
import random
from unittest.mock import Mock, patch


class Programmer:
    def __init__(self):
        self.tech_names = []

    def add_tech(self, tech_name):
        name = tech_name.lower()
        if name not in self.tech_names:
            self.tech_names.append(name)
        return self

    def get_random_tech(self):
        return random.choice(self.tech_names)


programmer = Programmer()
programmer.add_tech('python')
programmer.add_tech('java')
programmer.add_tech('sql')
programmer.add_tech('aws')
programmer.add_tech('django')

# 第一种写法
with patch('random.choice', return_value='python'):
    res = programmer.get_random_tech()
    print(res)

# 第二种写法
with patch('random.choice') as mock_random:
    mock_random.return_value = 'sql'
    print(programmer.get_random_tech())

# 第三种写法
@patch('random.choice')
def test_func(mock_random_choice):
	print(programmer.get_random_tech())

```

patch
```python
"""
在一个测试函数中 mock 一个 help 函数, 需要引入一个 mock 变量
"""
import unittest
from unittest.mock import patch
from code_generator import get_code

class TestGetCode(unittest.TestCase):
    @patch('random.randint', return_value=30)
    def test_get_code_mock_should_return_30(self, mock_random):
        self.assertEqual(get_code(), 'CX-30')
        
```


```python
"""
在一个测试函数中 mock 两个help 函数, 注意需要引入两个 mock 变量
"""
import unittest
from unittest.mock import patch
from code_generator import get_code_with_day

class TestGetCodeWithDay(unittest.TestCase):
    @patch('code_generator.get_code', return_value='CX-5')
    @patch('code_generator.get_today_name', return_value='FRI')
    def test_get_code_with_day_with_mocked_friday(self, mocked_get_code, mocked_get_today_name):
        actual = get_code_with_day()
        expected = 'CX-5-FRI'
        self.assertEqual(actual, expected)
        
```

mock object with setup
```python
import unittest
from unittest.mock import patch
from employees import Programmer

class TestProgrammer(unittest.TestCase):
	def setUp(self):
		self.programmer = Programmer()
		self.programmer.add_tech('python') \
			.add_tech('sql') \
			.add_tech('java') \
			.add_tech('c++') \
			.add_tech('aws')

	@patch.object(Programmer, 'get_random_tech') # 参数是类名和需要 mock 的方法名
	def test_get_random_tech_mocked_python(self, mock_tech):
		# mock_tech get_random_tech 方法, 定义了这个方法的返回值
		mock_tech.return_value = 'python'  
		# 因为上面定义了方法的返回值, 当项目调用方法的时候, 就会调用 mock_tech
		result = self.programmer.get_random_tech()
		self.assertEqual(result, 'python')
```


Request 测试用例
```python
"""
my_moudule.py
"""
import requests
import time


def get_content(url):
    retries = 3
    delay = 1
    for attempt in range(retries):
        try:
            response = requests.get(url)
            response.raise_for_status()
            return response.content.decode("utf-8")
        except Exception:
            time.sleep(delay)
    raise Exception(f"Failed to get content from {url}")

```

side effect
```python
from unittest.mock import MagicMock

mock_func = MagicMock()
mock_func.return_value = 10
print(mock_func())  # → 10

mock_func.side_effect = [1, 2, 3]
print(mock_func())  # → 1
print(mock_func())  # → 2
print(mock_func())  # → 3

```

```python
import unittest
from unittest.mock import MagicMock, patch
from my_module import get_content

class TestGetContent(unittest.TestCase):
	@patch("requests.get")
	def test_success(self, mock_get):
		mock_response = MagicMock(
			status_code=200, content=b"Hello, world!"
		)
		mock_get.return_value = mock_response
		result = get_content("http://example.com")
		self.assertEqual(result, "Hello, world!")
		mock_get.assert_called_once_with("http://example.com")

	@patch("requests.get")
	def test_retry(self, mock_get):
		mock_response = MagicMock(status_code=500, content=b"Hello, world!")
		mock_get.side_effect = [Exception, Exception, mock_response]
		result = get_content("http://example.com")
		self.assertEqual(result, "Hello, world!")
		self.assertEqual(mock_get.call_count, 3)
```

# Pytest

```shell
"""
Install Pytest
"""
pip install pytest
```

1. pytest的文件同样用`test_`开头
2. Pytest 不需要继承TestCase， 只要定义函数用`test_`开头
3. 函数中直接用Python关键词assert， 不像Unittest中用self.assert
4. 在一个测试函数中可以写多个assert 

```python
def test_foo():
	assert 1 == 1
	assert 2 == 2
```

### 对异常进行测试

```python
def test_configured_isis_with_value_error():  
    with pytest.raises(ValueError) as e_info:  # 指定某一种exception
        func()  # function will raise ValueError

```


### 创建公用实例

```python
import pytest

class MyClass:
	def g(self):
		return 2

@pytest.fixture
def c_instance():  # 类似工厂方法，用函数加fixture创建一个对象
	return MyClass()

def test_g(c_instance):  # 这里保证和fixture的函数定义一样
	assert c_instance.g() == 2    # 这里直接用c_instance作为实例
```

### setup & teardown

```python
import pytest

@pytest.fixture
def setup_teardown():
	print('setup')
	yield
	print('teardown')

# 第一种加载方式
def test_setup_teardown(setup_teardown):
	print('in test')

# 另一种加载方法
@pytest.mark.usefixtures('setup_teardown')  # 注意这里的参数是字符串
def test_g(c_instance):
	assert c_instance.g() == 2 

# 第三种加载方式，就是在定义fixture的时候配置autouse
# 则所有的测试函数都会隐性执行
@pytest.fixture(autouse=True)
def setup_teardown():
	print('setup')
	yield 
	print('teardown')
```

一个测试函数，可以接受多个fixture

# Run test

```shell
# 执行所有测试
pythom -m pytest 
# 压制 warning 执行所有测试
python -m pytest -p no:warnings
# 执行所有带有 read 的测试函数
python -m pytest -k read
```

# MonkeyPatching
```python

```


```shell

# run only the last failed tests
$ docker-compose exec web python -m pytest --lf

# run only the tests with names that match the string expression
$ docker-compose exec web python -m pytest -k "summary and not test_read_summary"

# stop the test session after the first failure
$ docker-compose exec web python -m pytest -x

# enter PDB after first failure then end the test session
$ docker-compose exec web python -m pytest -x --pdb

# stop the test run after two failures
$ docker-compose exec web python -m pytest --maxfail=2

# show local variables in tracebacks
$ docker-compose exec web python -m pytest -l

# list the 2 slowest tests
$ docker-compose exec web python -m pytest --durations=2
```