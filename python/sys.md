
`sys.path` 保存着当前 python 执行环境的目录 这是一个序列. 这个序列是保存在系统的环境变量`PYTHONPATH`中的.  而这个环境变量是在login的时候, shell 程序自动加载 `.profile` 时加载的. 
如果要引入一个本地的包, 可以在目录中添加一个路径
```
```python
print(sys.path)
```

在 linux 下查看所有的环境变量
```shell
env
```