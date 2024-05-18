
windows 系统的换行是 `\r\n` 如果在 window 上用文本编辑器写的 python 直接 copy 到 linux 上去运行， 就坑定运行不了
在 pycharm 有专门部署到 linux 的功能，会自动调整
在 linux 上安装 dos2unix
然后用 dos2unix filename 可以做转换

### 原始字符串
```python
r'c:\folder\file' # 保留原始字符串
'c:\\folder\file' # 转义
```


二进制

```python
b"\x71\x74" # 如果能把 71的 ascii 找到，就打印出 ascii 码， 如果找不到就直接打印 \x71

```
### format

固定长度字符串
```python
'| {:^10} | {:^10} |'.format(col1, col2)
```

/ mmk 