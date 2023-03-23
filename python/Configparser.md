
一般情况下， 把config文件命名为 `config.ini`

```text
[App]
tool_name = OSP NET Change
version = 1.2.1
```

调用代码
```python
import configparser

class Config:
	def __init__(self, setting_file_path):
		self.config = configparser.ConfigParser().read(setting_file_path)
		
	def get_config(self):
	    return self.config
	    
	def set_config(section, key, value):
	    self.config.set(section, key, value)
```

