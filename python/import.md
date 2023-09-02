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