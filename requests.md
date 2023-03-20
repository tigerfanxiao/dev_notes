# 获取session id完成验证

```python

import requests

session = requests.Session() # 创建一个session对象

# 获取cookies, 一般session id是存在cookie中的
session.cookies.get_dict()

```