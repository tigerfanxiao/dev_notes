首先要区分什么是CPU-bond和IO-bond
IO-bond 一般是等待外部的输入. 比如从本地硬盘读取文件, 从网络上下载文件, 获取数据库的链接等
CPU-bond 是需要本地cpu处理的计算, 比如处理本地图片, 数学计算等
线程Thread 一般是用于处理 IO-bond
进程Process 一般是处理处理 CPU-bond

同步和异步
同步是说, 在执行命令的过程中, 下一条指令必须等到前一条指令完成后才能执行
异步是说, 在前一条指令处于等待中, 比如因为网络延迟, 本地磁盘读取延迟时, 我们可以继续执行后面的命令. 

协程
用一个线程在多个任务之间循环, 找到需要被服务的任务
相比多进程, 节约资源, 但是比

python中的requests包目前只支持 Sync
原生支持 Async的包有 httpx, aiohttp, aiofile 
httpx 用于处理异步的网络请求
aiofile 用于处理异步的文件读取请求



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