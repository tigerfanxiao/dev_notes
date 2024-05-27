	
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

