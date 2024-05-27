
# `sys.path`

`sys.path` 保存着当前 python 执行环境的目录 这是一个序列. 这个序列是保存在系统的环境变量`PYTHONPATH`中的.  而这个环境变量是在login的时候, shell 程序自动加载 `.profile` 时加载的. 
如果要引入一个本地的包, 可以在目录中添加一个路径

```python

# 在模块运行完之后这个变量不会变化
sys.path.append('/test') # 添加一个模块的目录
print(sys.path)

sys.path.insert(1, '/sdf') # 插入在指定位置上
# 先要修改 sys.path
# 再 import
import thirdpartypackage

```

在 linux 下查看所有的环境变量
```shell
env
```


```python
sys.platform # 返回 linux 版本或者 windows
sys.version # python 的版本


```

# `sys.stdout()`

`print` is wrapper of `sys.stdout()`
