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



