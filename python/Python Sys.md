
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
import <thirdpartypackage>

```

在 linux 下查看所有的环境变量
```shell
env
```


```python
# 返回 linux 版本或者 windows
sys.platform 
# python 的版本
sys.version 

sys.modules # 返回模块名和具体路径的字典
list(sys.modules.keys())  # 模块名
sys.modules.get('datetime')  # 如果没有返回值表示没有加载这个模块


sys.argv # 显示由字符串组成的列表和命令行参数
sys.argv[1] # 参数引用是从1 开始的, 而不是 0

```

# 标准流

```python
sys.stdin
sys.stdout
sys.stderr

```
# `sys.stdout()`

`print` is wrapper of `sys.stdout()`


# 强行退出程序


```python
# 会直接将 python 程序终止, 之后的所有代码都不会继续执行
os._exit()

# 常用 , 会引发一个异常: SystemExit 如果这个异常没有被捕获, 那么 python 解释器将会退出. 如果有捕获此异常的代码, 那么这些代码还是会执行
sys.exit() 
```
