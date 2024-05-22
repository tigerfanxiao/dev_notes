# Null Value
```python
None
False
0
0.0
''
()
[]
{}
```

# Null Object
注意: 如果一个对象没有`__nonzero__()`和 `__len__()`两种方法, 则这个对象永远为真

```python
obj.__nonzero__() # 如果设置了这个方法, 且方法的返回值为True 则非空
# 如果没有定义 __nonzero__() 方法
obj.__len__() # 为空, 则对象为空
```


三元表达式

```python

value_true if a == a else value_false
```