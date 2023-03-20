
# Hidden Import


```
pyinstaller --onefile --hidden-import 'plugins.bard' .\example.py
```

# Hook

在python3.9的环境下
一定要确认本地的虚拟环境安装了这两个包
可以避免tinydb因不进去的我问题
```shell

pyinstaller==5.7.0  
pyinstaller-hooks-contrib==2022.15
```