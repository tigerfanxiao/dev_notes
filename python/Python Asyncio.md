协程
用一个线程在多个任务之间循环, 找到需要被服务的任务

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