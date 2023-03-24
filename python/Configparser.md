目前决定把`const.py`文件放在项目根目录下
如果在`const.py`中写 `__file__`那这个路径始终指向`const.py`
一般情况下， 把config文件命名为 `config.ini`

```text
[App]
tool_name = OSP NET Change
version = 1.2.1
```

调用代码
```python
import configparser

CONFIG_FILE_PATH = './config.ini'  # 注意这里不会校验文件是否存在
# 且用相对路径，在其他程序调用的时候会出问题
  
def get_config():  
    config = configparser.ConfigParser()  
    config.read(CONFIG_FILE_PATH)  # read没有返回值，是在原来的config上修改的
    return config


config = get_config()
# get_value
value = config[section][key]
# set key
config.set(section, key, value)  # 这个不会写如文本

# 这个写入文件
def set_config(section, key, value):  
    config = configparser.ConfigParser()  
    config.read(CONFIG_FILE_PATH)  
    config.set(section, key, value)  
  
    with open(CONFIG_FILE_PATH, 'w') as configfile:  
        config.write(configfile)
```


具体在写的时候，考虑用singleton

注意： 
1. 配置文件的路径如果写相对路径，在其他模块调用的时候会出问题
2. configparser模块不会校验配置文件是否存在，所以写代码自己校验

自己封装的config读取和配置方法

```python
import os  
import configparser  
  
class Config:  
    def __init__(self):  
        self.config_path = os.path.join(os.path.dirname(__file__), 'config.ini')  
        if not os.path.isfile(self.config_path):  
            raise FileNotFoundError('Config file does not exist')  
        self._config = configparser.ConfigParser()  
        self._config.read(self.config_path)  
  
    @property  
    def config(self):  
        return self._config  
  
    @config.setter  
    def config(self, section_key_value: tuple):  
        self._config[section_key_value[0]][section_key_value[1]] = section_key_value[2]  
        # 每次修改都会写入文件
        with open(self.config_path, 'w') as f:  
            self._config.write(f)  
  
  
if __name__ == '__main__':  
    config = Config()  
    print(config.config['token']['name'])  
    config.config = ('token', 'name', 'xiao')

```