re 是 python 的内置模块
# 正则表达式测试网站
https://regex101.com/


```
^  # 行首
$  # 行尾
.  # 任意字符, 除了换行符
\. # 点
?  # 出现 0 次或者 1 次
*  # 出现任意次数
\b # 单词分隔符
\D # 非数字
\d # 单个数字
\w # 大小写字符, 数字, 下划线
\W # 非大小写, 数字, 下划线
\s # 空格
\S # 非空格, 包括换行
[a-zA-Z] # 任意一个字母, 大小写不区分
[^a-z] # 任意非小写字符
\d # 整数
\d{1, 3} # 出现1 到 3 次
\d{2,}  # 至少出现 2 次
\d+ # 至少出现一次
.*  # 匹配一整行
root|Root # 匹配 root 或者 Root 中的一个

```

### `re.match()` & `groups()`
如果匹配不到, 则返回 `None`
```python
result = re.match(regex, content) # 如果是 none 就没有返回
result.groups() # 返回所有匹配中的结果, 是一个序列
```

### `group()`
```python
import re

pattern = r"(\d{4})-(\d{2})-(\d{2})"
string = "Today's date is 2024-10-12"

matched = re.search(pattern, string)

if matched:
    print(matched.group(0))  # Entire match: '2024-10-12'
    print(matched.group(1))  # First group: '2024'
    print(matched.group(2))  # Second group: '10'
    print(matched.group(3))  # Third group: '12'
```

### `re.findall()`
```python

import re
s = 'four digit 1234 five digits 56789 six digits 012345'
re.findall(r'\D(\d{5})\D?', s)  # ['56789', '01234']
```

注意： 
`findall`中不能使用 `^`或者`$`作为正则，因为匹配的可能是字符串的最末尾。如果要匹配字符串内的换行，用 `\n` 来匹配

### `re.search()`
只要能找到, 就返回非空
```python
re.search('[a-zA-Z]', password)
```

# Positive Lookahead

Positive lookbehind
```
(?=display|<End)
```

# 包含多行
一般正则表达式只会匹配到一行, 如果要正则表达式
```
[\s\S]*
```


### Named Capture Group

把匹配到的内容存入变量中
```python
import re
match = re.search('(?P<name>.*) (?P<phone>.*)', 'John 123456')
match.group('name') # 'John'

```

# 网络
```python
# 匹配一个 IP 地址
'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' 
```

### `re.split()`
相比字符串中普通的 split, 可以使用正则表达式来分割

```python

re.split('-+', 'fan---xiao------hello-word')
# ['fan', 'xiao', 'hello', 'word']

```

### `re.sub()`

```python
re.sub('-+', '##', 'fan---xiao------hello-word')
```