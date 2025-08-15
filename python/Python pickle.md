只能存小数据. 1 个 G 的数据立马死
### dump

```python
instance = MyClass()

with open(file_path, 'wb') as f:  
	pickle.dump(instance, f)  # 可以将任何对象序列化
```
### load

```python
with open(file_path, 'rb') as f:  
  
	instance = pickle.load(f)  # 读到就是对象
	
```