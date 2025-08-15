在 range 很大的时候, 会节约内存. 
```python
def my_range():
	 for i in range(3):
		 yield i


```


这些函数的作用对象都是list

# any

```python

any([False, False, True]) # True

```

# map

```python
# check if my_string is start with one of  ("Tun", "Loop", "NULL")
list(map(my_string.startswith, ("Tun", "Loop", "NULL") ))
```