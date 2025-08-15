模块搜索顺序
1. 当前目录
2. 环境变量
3. 标准链接库目录
4. 任何 `.pth`文件的内容
可以通过来[[Python Sys]]查看
### circular import error
在项目目录下有和公用模块同名的模块



如果 import 报错
```python
not_installed_modules = []

try: 
	from requests.auth import HTTPBasicAuth
except ImportError:
	not_installed_modules.append("requests")

try:
	import yaml
except ImportError:
	not_installed_modules.append("PyYAML")

if not_installed_modules:
	print("Please install following Python modules:")
	for module in not_installed_modules:
		print(" - {module}".format(module=module))
	sys.exit(1)
```