一般来说 IO 操作, 包括网络操作都要用 try, 因为网络连接都会有各种错误

不建议直接使用 except, 因为可能连 ctrl + C 都被捕获, 程序出错都无法退出

```python

try: 
	pass
except TypeError as mytype_err:
	print(str(mytype_err))
except Exception:
	pass 
else: 
	# 没有错误
finally:
	# 总是会执行
```