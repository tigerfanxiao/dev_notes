# 问题
1. 怎么确定测试覆盖范围的报告
2. 找一些 assert_called, assert_called_once 这些的练习
3. 模拟数据库不能访问的测试函数
4. 了解一些Integration test的内容
5. 发送邮件的测试函数, patch mock的例子

# 概念
黑盒白盒测试 black-box testing and white-box testing
- 黑盒是不懂程序内部结构的, 只知道输入和预期的返回值.
- 白盒是了解程序内部架构的
# Unit test

执行某个特定的测试方法
```shell
# MyTestCase 是某个TestCase, test_data 是def test_data(self) 方法
python -m unittest MyTestCase.test_data
```
测试用例写法
1. 先定义 TestCase类
2. 再定义test_ 方法

```python
import unittest

def add(x, y):
	return x + y

class TestAddition(unittest.TestCase):
	def test_add_positive_numbers(self):
		result = add(2, 3)
		self.assertEqual(result, 5, msg=None)

if __name__ == '__main__':
	unittest.main()
```

各种assert 方法
```python
# 对比两个序列
self.assertListEqual(result, expected) 
# 不等于 None
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
# 上面的表达式等同于
with self.assertRaises(TypeError):
	callable(*args, **kwargs)
	
# 是否在容器中
self.assertIn(member, container, msg=None)
self.assertNotIn(member, container, msg=None)

# 在比较浮点数计算, 查看到小数点后2位
self.assertAlmostEqual(first_float, second_float, places=2, delta=0.5)
	
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
### mock

当我们的代码中有类似request, 调用远端api, 需要访问网络这种情况时, 因为网络范围有很多不确定性,所以可以利用mock来处理
下面举个简单的例子
```python
# 文件名是 main.py
import requests  
# 只是需要被测试的函数 len_joke
def len_joke():  
    joke = get_joke()  
    return len(joke)  

# 这里是被依赖的函数, 因为是API调用, 我们要mock的是这个函数
def get_joke():  
    url = 'http://api.icndb.com/jokes/random'  
    response = requests.get(url)  
    if response.status_code == 200:  
        joke = response.json()['value']['joke']  
    else:  
        joke = 'No Jokes'  
    return joke
```
测试函数的写法
```python
import unittest

from unittest.mock import patch
# 先吧需要测试的函数引入
# 注意这里不一定需要引入 get_joke 因为这个被mock的函数是patch装饰器去找的
from main import len_joke

class TestLenJoke(unittest.TestCase):
	@patch('main.get_joke') # 这里main是module的名字, get_joke是对应的函数
	def test_len_joke(self, mock_get_joke) # 这里是说我们要用mock_get_joke整个函数来替代原来的get_joke函数
		mock_get_joke.return_value = 'one' # 这里我们定义了这get_joke函数的返回值
		# 我们假设在实际运行len_joke时, 依赖的get_joke返回值是 "one", 所以len_joke的返回值是3
		self.assertEqual(len_joke(), 3)

if __name__ == '__main__':
	unittest.main()
```
MagicMock 的作用
当我们patch一个函数的时候, 如果整个函数的返回值是一个有复杂架构的实例对象, 比如一个HTTP请求的返回值response是一个有复杂结构的对象. 那么模拟整个对象是需要先构造一个类的, 再通过实例化, 才能生成一个模拟的对象. 所以在unittest框架中提供了MagicMock 这个类. 用于快速构建一个复杂结构的实例. 我们还是用上面的例子举例
```python
import unittest
from unittest.mock import patch, MagicMock
from main import get_joke # 这是这次需要被测试的函数

# 对get_joke 函数进行测试
class TestLenJoke(unittest.TestCase):
	# 注意命名空间, 这里是main模块下的引入的的requests包, 不是全局的requests包
	# 这里不是patch了一个requests.get方法, 而是直接patch requests整个模块
	@patch('main.requests') 
	def test_get_joke(self, mock_requests): # 这里mock的是整个requests 整个模块
		# 我们现在来构造一个复杂对象作为 mock_requests.get.return_value 的返回值
		mock_response = MagicMock(status_code=200)
		# 这里根究原代码 json也是一个函数, 所以有返回值 return_value
		mock_response.json.return_value = {
			'value': {'joke': 'one'}
		}
		mock_requests.get.return_value = mock_response
		self.assertEqual(get_joke(), 'one')

	# 对于获取数据失败的测试
	@patch('main.requests')
	def test_fail_get_joke(self, mock_requests):
		mock_response = MagicMock(status_code=403)
		mock_requests.get.return_value = mock_response
		self.assertEqual(get_joke(), 'No Jokes')
```
注意: 如果有需要`@patch()` 可以叠加使用

patch 实例中的方法
类的定义
```python
import random

class Programmer:
    def __init__(self):
        self.tech_names = set()

    def add_tech(self, tech_name):
        name = tech_name.lower()
        if name not in self.tech_names:
            self.tech_names.add(name)
        return self
	# 这里有随机值, 所以用patch方法来替代
    def get_random_tech(self):
        if not self.tech_names:
            raise ValueError('No technologies added yet')
        return random.choice(list(self.tech_names))

```


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
            
	# 这里path.object中是使用的是类, 类中的方法是 get_random_tech
    @patch.object(Programmer, 'get_random_tech')
    def test_get_random_tech_mocked_python(self, mock_tech):
	    # 将整个实例中的方法定义了返回值
        mock_tech.return_value = 'python'
        # 在通过实例调用方法来实现mock
        actual = self.programmer.get_random_tech()
        expected = 'python'
        self.assertEqual(actual, expected)
```




```python
from unittest.mock import Mock, MagicMock, patch
# Create a basic mock object
mock_obj = Mock(name='first_mock')
# attribute
mock_obj.call_count
# 基于某个类构建实例
weather_api = mock.MagicMock(spec=WeatherAPI)
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
mock_obj.assert_called() # 如果某个mock对象的方法被调用了
mock_obj.assert_called_once() # 如果这个mock对象, 被某个函数作为参数调用了
mock_obj.assert_called_with(1, 2) # 被调用的参数
mock_obj.assert_called_once_with(1, 2) # 被调用一次
self.assertEqual(mock_obj.call_count, 3) # 被调用3次
mock_obj.assert_not_called()
```
assert has calls
email client 
```python
import smtplib


class EmailClient:
    def __init__(self, email, password):
        self.email = email
        self.password = password

    def send_email(self, recipient, subject, body):
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login(self.email, self.password)
        message = f"Subject: {subject}\n\n{body}"
        server.sendmail(self.email, recipient, message)
        server.quit()


class EmailSender:
    def __init__(self, email_client):
        self.email_client = email_client

    def send_emails(self, recipients, subject, body):
        for recipient in recipients:
            self.email_client.send_email(recipient, subject, body)

```
写一个email_sender 的测试
```python
import unittest
from unittest import mock
from email_client import EmailClient, EmailSender


# Enter your solution here
class TestEmailSender(unittest.TestCase):
    def test_send_emails(self):
	    # 这里mock的对象是 email_client, 所以这个对象的 send_email是mocked function
        email_client = mock.MagicMock(spec=EmailClient)
        recipients = [
            "recipient1@example.com",
            "recipient2@example.com",
        ]
       
        email_sender = EmailSender(email_client)
         # 这里是执行了函数, 函数会被执行两次
        email_sender.send_emails(
            recipients, "Test email", "This is a test email"    
        )
        # 这里要检验被mocked function 被调用了2次
        email_client.send_email.assert_has_calls(
            [
                mock.call(
                    "recipient1@example.com",
                    "Test email",
                    "This is a test email"
                ),
                mock.call(
                    "recipient2@example.com",
                    "Test email",
                    "This is a test email",
                ),
            ]    
        )
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

# parameterized

### subTest
```python
import unittest
from rectangle import area, perimeter

class TestArea(unittest.TestCase):
	def test_area(self):
		test_cases = [
			(1, 5, 5),
			(2, 10, 20),
			(100, 50, 5000)
		]
		for width, height, expected in test_cases:
			with self.subTest(
				width=width, height=height, expected=expected
			):
				self.assertEqual(area(width, height), expected)
```

### parameterized
```shell
pip install parameterized
```

```python
import unittest  
from parameterized import parameterized  
  
def add(x, y):  
    return x + y  

class TestAddition(unittest.TestCase):  
    @parameterized.expand([  
        (1, 2, 3),  
        (0, 0, 0),  
        (-1, 1, 0),  
        (10, -5, 5),  
    ])  
    def test_addition(self, a, b, expected_result):  
        result = add(a, b)  
        self.assertEqual(result, expected_result)
```

how parameterized to handle Exception
```python
import unittest  
from parameterized import parameterized  
  
def average(numbers):  
    if not isinstance(numbers, list):  
        raise TypeError("Input must be a list")  
    if len(numbers) == 0:  
        raise ValueError("List must not be empty")  
    if not all(isinstance(n, (int, float)) for n in numbers):  
        raise TypeError("All input must be numbers")  
    return sum(numbers) / len(numbers)  
  
class TestAverage(unittest.TestCase):  
    @parameterized.expand([  
        ([1, 2, 3], 2),  
        ([10, 20, 30, 40], 25),  
        ([5], 5),  
        ([0, 0, 0], 0),  
        ([], None),  
        (["not a number"], None),  
        (["1", 2, 3], None),  
        ([None], None)  
    ])  
    def test_average(self, numbers, expected):  
        if expected is None:  
            with self.assertRaises((TypeError, ValueError)):  
                average(numbers)  
        else:  
            result = average(numbers)  
            self.assertEqual(result, expected)
```

# test suites
组织Test, 来构建特点的Scenario

```python
import unittest

# 把某个TestCase中的方法加到 suite中
suite = unittest.TestSuite()
suite.addTest(MyTestCase('test_method1'))
suite.addTest(MyTestCase('test_method2'))
```
自动发现测试方法, 加到suite
```python
# 在一个suite加入多个TestCase
suite.addTest(unittest.makeSuite(TestAdditionAndSubtraction))
suite.addTest(unittest.makeSuite(TestMultiplicationAndDivision))
```


```python
import unittest
# Create a test loader
loader = unittest.TestLoader()
# 把 TestCase 下面的所有测试方法都加入 suite
suite = loader.loadTestsFromTestCase(MyTestCase)
# Create a test runner and run the suite
runner = unittest.TextTestRunner()
runner.run(suite)
```
那多个suite 有顺序的组合在一起
```python
import unittest

# Create test loaders and load test cases
loader1 = unittest.TestLoader()
suite1 = loader1.loadTestsFromTestCase(MyTestCase1)

loader2 = unittest.TestLoader()
suite2 = loader2.loadTestsFromTestCase(MyTestCase2)

# Create a master test suite containing the individual test suites
master_suite = unittest.TestSuite([suite1, suite2])
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