Requests包难以完成页面跳转登录的功能，可能selenium更适合

### Response属性

```python
import requests
response = requests.get('url')
# Headers
response.headers  
# 返回值的 json 格式
response.json() 
# 是否是 200
response.ok
# 出先 error 的理由, 比如 url not found
response.reason
# HTTP状态码. 比如 200, 404 等
response.status_code
# 返回的数据文本
response.text
# 如果 status code 不是 200 会爆出异常
response.raise_for_status
```


### 规避SSL Error
```python

import requests 


# 配置verify来规避https ssl 问题
r = requests.get('https://netlive-de.gts.huawei.com/fbb', verify=False)  

```

### 规避 Authentication Warning
```python
import json
import requests   
from requests.auth import HTTPBasicAuth
requests.packages.urllib3.disable_warnings() 

AUTH = HTTPBasicAuth('cisco', 'cisco') # 输入验证
HEADERS = {'Accept': 'application/yang-data+json',
            'Content-Type': 'application/yang-data+json' }
            
URL = 'https://csr1v1/restconf/data/Cisco-IOS-XE-native:native'
# 用 get 方法
response = requests.get(URL, headers=HEADERS, auth=AUTH, verify=False)
# 用 post 方法, payload 放在 data 参数中
esponse = requests.post(url, data=json.dumps(payload), headers=headers, auth=auth, verify=False)
```