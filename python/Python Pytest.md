# Questions

using fixtures to set up your tests
how to mock external dependencies

- [ ] 读取json文件
# Install
```shell
pip install pytest
```
# Pytest VS Unittest
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