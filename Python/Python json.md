
Json其实有两种主要的形式
最外层是字典的

```json
{
	"device_name": "R1",
	"device_type": "Router"
}
```

最外层是列表的
```json
[
	{
		"device_name": "R1",
		"device_type": "Router"
	}, 

	{
		"device_name": "R2",
		"device_type": "Router"
	}
]
```

### Serialization

序列化: 将 python 中的对象, 比如序列和字典转变为字符串

```python
import json

one_python_obj = { "name": "xiao", "age": 20}
json.dumps(one_python_obj) # 没有缩进
json.dumps(one_python_obj, indent=4) # 添加缩进

```

存储到文件
```python
json.dump(one_python_obj, file_handler, indent=4) # 写入文件
```
### Deserialization
反序列化: 将文本发序列化为 python 对象
```python
import json

# 用于with中读取file handler
json.load(open(file='./config.json', encoding='utf-8'))  

# 用于直接读取json的字符串
json_str = '{"name": "xiao", "age": 20}'
my_dict = json.loads(json_str)  
```