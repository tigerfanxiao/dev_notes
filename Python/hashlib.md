
```python
from hashlib import blake2b

# 对密码进行加密保存
blake2b(new_password.encode('UTF-8')).hexdigest()

```


做hash值
```python
hash(st)
```