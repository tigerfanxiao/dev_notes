# Context Manager

  Customized context manager 的构造方法
```python
#!/usr/bin/env python3  
# -*- coding=utf-8 -*-  
  
import time  
class Timer:  
    def __enter__(self):  
        self.start = time.perf_counter()  
        print("Starting timer.....")  
        # Optional, return 的对象作为 with 的对象可以使用.  
        # 注意这里能返回的对象只有是在__enter__中定义的, 并操作的  
        # 在__exit__中的定义的和修改的是在 Context Manager 执行完毕退出后才执行的  
        return self  
  
  
    def __exit__(self, exc_type, exc_val, exc_tb):  
        # 这里打印的是如果 content manager wrap 的代码发生错误, 做 exception 处理的类型, 对象, traceback 信息  
        print(exc_type, exc_val, exc_tb)  
        self.end = time.perf_counter()  
        self.elapsed = self.end - self.start  
        print(f"Elapsed time: {self.elapsed:.6f}")  
  
if __name__ == '__main__':  
  
    # Exception 不处理的话, 会退出程序  
    with Timer() as t:  
        print(1 / 0)  
  
    # 没有 Exception, 但是因为上面代码出现了异常, 就没有执行  
    with Timer() as t:  
        print(t.start) #  
        total = sum(range(1_000_000))  
        print(f"Total time: {total}")
```

context manager 的错误处理
```python
#!/usr/bin/env python3  
# -*- coding=utf-8 -*-  
  
import time  
class Timer:  
    def __enter__(self):  
        """这个例子说明了 customized context manager 要怎么写"""  
        self.start = time.perf_counter()  
        print("Starting timer.....")  
        # Optional, return 的对象作为 with 的对象可以使用.  
        # 注意这里能返回的对象只有是在__enter__中定义的, 并操作的  
        # 在__exit__中的定义的和修改的是在 Context Manager 执行完毕退出后才执行的  
        return self  
  
  
    def __exit__(self, exc_type, exc_val, exc_tb):  
        # 这里打印的是如果 content manager wrap 的代码发生错误, 做 exception 处理的类型, 对象, traceback 信息  
        print(exc_type, exc_val, exc_tb)  
        self.end = time.perf_counter()  
        self.elapsed = self.end - self.start  
        print(f"Elapsed time: {self.elapsed:.6f}")  
  
if __name__ == '__main__':  
  
    # Exception 不处理的话, 会退出程序  
    with Timer() as t:  
        print(1 / 0)  
  
    # 没有 Exception, 但是因为上面代码出现了异常, 就没有执行  
    with Timer() as t:  
        print(t.start) #  
        total = sum(range(1_000_000))  
        print(f"Total time: {total}")
```
# Pycharm
删除一行
ctrl + y

复制一行
ctrl + d

移动一行
ctrl + shift + arrow

vertical Select
alt + shift + insert 退出的化，再操作一次

快速修复
alt + Enter

选中单词
ctrl + w

# windows
alt + F12 打开 terminal
esc 回到代码编辑窗口


# mac
### 代码编辑
option + up 选中整个单词
ctrl + G 选中重复出现的单词, 每按一次 G 会向下查找
option + delete 删除一个单词
cmd + delete 删除完整一行

### 窗口控制
ctrl + tab 在打开的标签页中切换


### 代码跳转
cmd + b 跳转到变量定义的地方


# Terminal

在 Terminal 下面运行 python 解释器是在虚拟环境里

### 可以在虚拟环境中加载某一个 .py文件

```shell
python -i  module.py
```


### 把目录引入 sys.path

![[Pasted image 20240526230710.png]]

### python 脚本模板
只有在 Linux 下生效

- 指定解释器
- 指定编码

```python
#!/usr/bin/env python3
# -*- coding=utf-8 -*-

# 指定 python3 为解释器, 可以直接执行, 需要给文件执行权限 chmod +x file.py
# 在目录下 ./file.py 直接运行. 
# 可以用 python3 file.py 执行, 手动指定解释起
# 保证中文不出现乱码

if __name__ == '__main__':
    pass
```

![[Pasted image 20240526233255.png]]


在 pycharm 中 module 和 package 可以互转

