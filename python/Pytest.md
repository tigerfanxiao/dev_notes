# Questions

using fixtures to set up your tests
how to mock external dependencies

- [ ] 读取json文件
- [ ] 
# Install
```shell
pip install pytest
```
# Pytest VS Unittest
1. pytest的文件同样用`test_`开头
2. 不需要继承TestCase， 只要定义函数用`test_`开头
3. 函数中直接用Python关键词assert， 不像Unittest中用self.assert
4. 在一个测试函数中可以写多个assert 
```python
def test_foo():
	assert 1 == 1
	assert 2 == 2
```


### Test Exception

```python
def test_configured_isis_with_value_error():  
    with pytest.raises(ValueError) as e_info:  # 指定某一种exception
        func()  # function will raise ValueError

```

# Fixture

### 创建公用实例
```python
import pytest

class MyClass:
	def g(self):
		return 2

@pytest.fixture
def c_instance():
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
### fixture 的继承
