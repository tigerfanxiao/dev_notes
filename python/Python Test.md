
# Unit test
```python
import unittest

def add(x, y):
	return x + y

class TestAddition(unittest.TestCase):
	def test_add_positive_numbers(self):
		result = add(2, 3)
		self.assertEqual(result, 5)
		
	def test_add_negative_numbers(self):
		result = add(-2, -3)
		self.assertEqual(result, -5)	

if __name__ == '__main__':
	unittest.main()
```

asset 只是用于开发环境, 不用于生产环境
```python 
countries = ['POL', 'ENG', 'GER', 'USA', 'ITA']
is_italy = 'ITA' in countries

assert is_italy, 'Italy not found in the countries list.'
```
### setup in unitest



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
# Configure the mock to return a specific value when called
mock_obj.return_value = 42

# Configure the mock to raise an exception when called
mock_obj.side_effect = Exception("Mocked Exception")

# Inspect how the mock was called
mock_obj.assert_called_with(1, 2, keyword_arg="value")
```
Using Patching for Context Manager: To temporarily replace an object or function with a mock during a specific scope of code, you can use the patch context manager.

```python
from unittest.mock import patch
with patch('module_name.function_name', new=mock_obj):
	# Code that uses the mocked function
```

- **Verifying Calls**: You can use various methods like `**assert_called_with**`, `**assert_called_once_with**`, and `**assert_not_called**` to verify how the mock object was called during the test.
```python
mock_obj.assert_called_with(1, 2)
mock_obj.assert_called_once_with(1, 2)
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

# Fixture
Fixture的作用
1. 创建公共的实例
2. 创建setup和teardown

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