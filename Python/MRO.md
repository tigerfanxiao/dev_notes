Method Resolution Order
```python
class Alpha: 
	def __str__(self): 
		return 'alpha' 
		
class Beta: 
	def __str__(self): 
		return 'beta' 

class C(Alpha, Beta): 
	def str(self): # 本地没有定义 __str__, 需要从Alpha中去找, Beta没有涉及
		pass 

o = C() 
print(o) # 返回 alpha, 
```

