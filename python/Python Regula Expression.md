

```
^  # 行首
$  # 行尾
\D # 非数字
\d # 单个数字
```
### re.findall()
```python

import re
s = 'four digit 1234 five digits 56789 six digits 012345'
re.findall(r'\D(\d{5})\D?', s)  # ['56789', '01234']
```

注意： 
`findall`中不能使用 `^`或者`$`作为正则，因为匹配的可能是字符串的最末尾。如果要匹配字符串内的换行，用 `\n` 来匹配


# 向前看和向后看

```
(?=display|<End)
```

包含多行
```
(.|\n)*?)
```


### Named Capture Group

把匹配到的内容存入变量中
```python
import re
match = re.search('(?P<name>.*) (?P<phone>.*)', 'John 123456')
match.group('name') # 'John'

```