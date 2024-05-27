推荐使用 f-string, 当变量很多的时候, 不用数是第几个位置变量
# 老的写法 %

```python

'%10d'%(9) # 10d 表示占 10 个字符. 在左面补齐空格
'%-10.2f'%(3.1415926) # 表示右边补齐空格, 且取两位小数
'%06.2f'%(3.1415926) # 在左面补齐 0

```

# Format 写法

```shell
# {0} 表示第一个变量
'{0}, {2}, {1}'.format(3.1415926, 2, 1)
# 如果不写位置, 默认是从第一个, 第二个, 第三个变量递推的
'{}, {}, {}'.format(1, 2, 3)

# 给位置变量命名
'{var1}, {var2}, {var3}'.format(var1=1, var2=2, var3=3)
# 第0个位置变量宽度为 10, 做边补空格
'{0:10}'.format(0, 1)

# 浮点数
'{0:.2f}'.format(3.1415)

'{0:>10}'.format(1)  # 数字在右边, 左面补齐
'{0:<10}'.format(1)  # 数字在左边, 右面补齐
'{0:^10}'.format(1)  # 数字在中间, 中间对其

# 可以直接取字典中的 key
'{0[key]:^10}'.format(dict(key='name'))


```

# f-string

```shell
f'{var1}'
```