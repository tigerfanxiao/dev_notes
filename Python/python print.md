
默认清库下 print 的内容是会放在 buffer 里的, 也就是 buffer 满了才会一次性打印出来. 但是用 flush 参数, 可以强制要求 buffer 不留存, 全部输出

```python
print("ddd", flush=True)
```


```python

# 不打印最后一行
print('xiao', end='') 
# 把打印出来的内容直接放到文件中
print('xiao', file=open('test.txt', 'w', encoding='UTF-8'))
```