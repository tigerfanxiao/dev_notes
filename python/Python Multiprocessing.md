
```python

from multiprocessing import cpu_count, Pool as ProcessPool
from multiprocessing.pool import ThreadPool
from multipprocessing import freeze_support


if __name__ == '__main__':
	# 多进程
	freeze_support() # windows 平台要加上这句, 并且一定要放在 if __name__=='__main__' 后面才能避免 runtime
	pool = ProcessPool() # 有效控制并发进程或者线程数, 默认为内核数(推荐)
	cpus = cpu_count() # 得到内核数的方法
	print(cpus)
	# 多线程
	pool = ThreadPool(100)

	results = []
	for i in range(0, 10):
		x = random.randint(1, 10)
		y = random.randint(1, 10)
		z = random.randint(1, 10)
		result = pool.apply_async(qyt_multi, args=(x, y, z))
		results.append(result)


	pool.close()
	pool.join() # 调用 join 之前. 先调用 close 函数, 否则会出错. 执行完 close 后不会有新的进程加入 pool, join

	for i in results:
		print(i.get())
```