
pickle 是把一个文件全部读进入内存
selves 也是把一个大文件, 放了很多键, 把大的 pickle 分割成很多小的部分. 变成一个小的文件系统


```python
# 通过文件写入数据

with shelve.open('./pickle_dir/people-shelve') as f:
	f['cq_bomb'] = cq_bomb
	f['tina'] = tina
	f['ender'] = ender
	f['datetime'] = datetime.now()

# 读取文件

with shelve.open('./pickle_dir/people-shelve') as f:
	for k, v in f.items():
		print(k,v )
	
```
