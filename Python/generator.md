在读取巨大海量的文件中使用
```python

def gen(n): 
	i = 0
	while i < n: # 使用while
		yield i
		i += 1
		
gen_5 = gen(5)
for i in gen_5:
	print(i)
	

def gen(n):
	for i in range(n):
		yield i

list(gen(5))

```


```python
# 逐行读取，不一次性加载整个文件 
def read_big_file(path): 
	with open(path, "r", encoding="utf-8") as f: 
		for line in f: 
			yield line # 每次读取一行数据
```