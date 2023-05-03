Requests包难以完成页面跳转登录的功能，可能selenium更适合


### 规避SSL Error
```python

import requests 

# 配置verify来规避https ssl 问题
r = requests.get('https://netlive-de.gts.huawei.com/fbb', verify=False)  
print(r.url)

```