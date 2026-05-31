首先要区分什么是CPU-bond和IO-bond
IO-bond 一般是等待外部的输入. 比如从本地硬盘读取文件, 从网络上下载文件, 获取数据库的链接等
CPU-bond 是需要本地cpu处理的计算, 比如处理本地图片, 数学计算等
线程Thread 一般是用于处理 IO-bond
进程Process 一般是处理处理 CPU-bond

同步和异步
同步是说, 在执行命令的过程中, 下一条指令必须等到前一条指令完成后才能执行
异步是说, 在前一条指令处于等待中, 比如因为网络延迟, 本地磁盘读取延迟时, 我们可以继续执行后面的命令. 

# 协程 COROUTINES
首先一个.py 中asyncio.run 在主线程中构建了一个event loop, main 是这个event loop中定义的第一个协程, 但是当遇到main中嵌套定义的await时候, main这个协程会被suspend, 把控制权交给event loop, event loop 会看这个await任务能否完成, 如果可以完成, 则玩出后回到event loop, 如果当前await 任务不能完成, 则又会suspend, 去到下一个await任务上. event loop始终在运行, 知道所有的await任务都完成, 最后回到主线程上

区分几个重要概念
- Coroutines 通过 async def func 完成协程函数的申明
- Task 编排和调用Coroutine, 把Coroutine注册到event loop中并运行
- Future 一般不会直接操作的 low-level 对象, 用于存放task的结果和状态(complete/pending/exception)


python中的requests包目前只支持 Sync
原生支持 Async的包有 httpx, aiohttp, aiofile 
httpx 用于处理异步的网络请求
aiofile 用于处理异步的文件读取请求

如何改造 async 函数
1. 在定义一个函数时, 加入 async 关键词
2. 在async内部使用await关键词, 调用awaitable 对象

```python
import asyncio

# 这里定义了一个异步函数 fetch_data，它模拟了一个网络请求，等待一段时间后返回一些数据。
async def fetch_data(id, delay=1):
	print(f"Fetching data... ID: {id} with delay {delay} seconds")
	await asyncio.sleep(delay) # Simulate a network delay
	print(f"Data fetched! ID: {id}" )
	return {"data": f"Sample data for ID: {id} with delay {delay} seconds"}

# main函数时为了控制多个协程使用的, 因为要调用await, 所以也需要用async定义
# 这里也是调用了协程的, 运行时间在2s
async def main():
	# 注意只有用了 create_task 才能利用协程的能力, 如果只是 await fetch_data()则无法使用协程的能力
	task1 = asyncio.create_task(fetch_data(1, 2)) 
	task2 = asyncio.create_task(fetch_data(2, 1)) 
	result1 = await task1
	result2 = await task2
	
	print(result1, result2)

asyncio.run(main())

'''
Fetching data... ID: 1 with delay 2 seconds # 首先调用的是任务1
Fetching data... ID: 2 with delay 1 seconds
Data fetched! ID: 2 # 最先被打印结果的是任务2, 因为delay短
Data fetched! ID: 1
# main函数最后打印语句是等两个task都完成后才打印的
{'data': 'Sample data for ID: 1 with delay 2 seconds'} {'data': 'Sample data for ID: 2 with delay 1 seconds'}
'''
```

gather
```python
# 使用gather 把所有认为的返回结果放在一个list里面, 直到所有的协程运行完毕
# gather 的缺点在于内部没有较好的异常处理机制, 如果有一个任务失败了, 不会取消别的任务
results = await asyncio.gather(
	fetch_data(1, 2),
	fetch_data(2, 1)
) 
```

taskgroup
```python
tasks = []
# 
async with asyncio.TaskGroup() as tg:
	for i, sleep_time in enumerate([2,1], start=1):
		task = tg.create_task(fetch_data(i, sleep_time))
		tasks.append(task)
		
	# 使用.result() 把结果取出来
	results = [task.result() for task in tasks]
```
lock
确保资源在同一个时刻只被一个协程访问
```python
import asyncio

shared_resource = 0

async def modify_shared_resource(lock):
	global shared_resource
	async with lock:
		print("Modifying shared resource...")
		shared_resource += 1
		await asyncio.sleep(1) # Simulate some work
		print(f"Shared resource value: {shared_resource}")

async def main():
	lock = asyncio.Lock()
	await asyncio.gather(*(modify_shared_resource(lock) for _ in range(5)))

  
asyncio.run(main()) # 这里构建了一个event loop, 在main内部的其他task或者lock会被放入同一个event loop
```

semaphore
限制只能有固定数量的协程在运行
```python
import asyncio

async def access_resource(semaphore, resoucece_id):
	async with semaphore: # 接受一个semaphore
		print(f"Accessing resource {resoucece_id}...")
		await asyncio.sleep(1)
		print(f"Finished accessing resource {resoucece_id}.")

async def main():
	semaphore = asyncio.Semaphore(2) # 构建一个Semaphore 对象
	tasks = [access_resource(semaphore, i) for i in range(5)]
	await asyncio.gather(*tasks)

if __name__ == "__main__":
	asyncio.run(main())
```

event
可以手动指定在某个任务结束完成后 setter, 再继续执行某个动作
```python
import asyncio

async def waiter(event):
	print("Waiting for the event to be set...")
	await event.wait()
	print("Event is set! Continuing execution...")
	
async def setter(event):
	print("Setting the event in 3 seconds...")
	await asyncio.sleep(3)
	event.set()
	print("Event has been set.")

async def main():
	event = asyncio.Event()
	await asyncio.gather(waiter(event), setter(event))

if __name__ == "__main__":
	asyncio.run(main())
```

```python
import requests
import asyncio
import os
import threading

loop = asyncio.new_event_loop()
asyncio.set_event_loop(loop)

async def pressure_test(host, task_id):
	print(f'ID: {task_id} started')
	print(os.getpid(), threading.current_thread().ident)
	# awaitable loop.run_in_executor(executor, func, *args)
	# requests.get 这个位置是函数
	# host
	result = await loop.run_in_executor(None, requests.get, host)
	print(f'ID: {task_id} Stopped
	return result.status_code


		  
def pressure_test_main(conns, url):
	# 任务列表
	tasks = []
	# 创建任务并放入 tasks 列表

	for i in range(conns):
		task = loop.create_task(pressure_test(url, i))
		tasks.append(task)

	# 等待任务完成
	loop.run_until_complete(asyncio.wait(tasks))

	# 使用 .result() 提取任务结果
	result_list = []
	for i in tasks:
		result_list.append(i.result())

if __name__ == '__main__':
	print(pressure_test_main(5, 'https://www.baidu.com'))
	

```