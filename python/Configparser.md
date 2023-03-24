
一般情况下， 把config文件命名为 `config.ini`

```text
[App]
tool_name = OSP NET Change
version = 1.2.1
```

调用代码
```python
import configparser

CONFIG_FILE_PATH = './config.ini'  
  
def get_config():  
    config = configparser.ConfigParser()  
    config.read(CONFIG_FILE_PATH)  # read没有返回值，是在原来的config上修改的
    return config


config = get_config()
# get_value
value = config[section][key]
# set key
config.set(section, key, value)

```

