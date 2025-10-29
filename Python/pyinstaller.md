不需要装 python 环境, 可以直接给 windows 用户使用

```shell
pip install pyinstaller

pyinstaller --onefile -w .\example.py

# 生成的exe 文件在 dist 目录下
```


# App 路径问题

```python
def get_app_running_path():
	if getattr(sys, 'frozen', False):
	    return os.path.dirname(sys.executable)  # 如果是exe运行
	else:
		return os.path.dirname(__file__)  # 如果是本地调试
```

# Hidden Import
```
pyinstaller --onefile --hidden-import 'plugins.bard' .\example.py
```

# Hook

在python3.9的环境下
一定要确认本地的虚拟环境安装了这两个包
可以避免tinydb因不进去的问题
```shell

pyinstaller==5.7.0  
pyinstaller-hooks-contrib==2022.15
```