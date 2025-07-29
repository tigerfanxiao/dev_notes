
Delay
```python
import requests
import time

def get_content(url):
    retries = 3
    delay = 1
    for attempt in range(retries):
        try:
            response = requests.get(url)
            response.raise_for_status()
            return response.content.decode("utf-8")
        except Exception:
            time.sleep(delay)
    raise Exception(f"Failed to get content from {url}")

```

Requests包难以完成页面跳转登录的功能，可能selenium更适合

### Response Attributes

```python
"""
respose 有很多常见的属性
"""
import requests
response = requests.get('www.google.com') # 得到 200
response.status_code # HTTP 状态码
response.text # decode过的
response.content # byte 字节, 未decode
response.json() # 如果是API调用, 则用json解码, 返回值已经修改为字典
response.raise_for_status # 如果 status code 不是 200 主动报异常

# Headers
response.headers  
# 是否是 200 返回 Boolean
response.ok
# 出先 error 的理由, 比如 url not found
response.reason
```


### Authentication
在调用 API 时, 一般有三种验证方式
1. Basic Authentication 是基于 Base64 编码成一组字符串
2. 基于 Session, 就是在 cookie 中放一个 sessionID 之类的
3. 基于 token 的, 就是API 服务器给一个动态 token

#### Base64验证
base64 编码生成网站
base64encode.org
使用 python 生成 Base64 秘钥
```python
import base64
# 将用户名和密码组一起编码
auth = "cisco:cisco".encode() 
# base64 再编码一次, 这里变量必须是 bytes, 不是 string 格式的
basic_auth = base64.b64encode(auth)

# 或者用下面的一步到位
basic_auth = base64.b64encode(bytes(u'cisco:cisco', "utf-8"))

# 还要把 basic_auth反编译回来
basic_auth = basic_auth.decode()

# 注意, 在发送验证时, 前面要带上 Basic
HEADERS = {"Accept":"application/yang-data+json", "Authorization" : "Basic {}".format(basic_auth)}

```

#### Token 验证
```python
import requests
DNA_URL = "https://sandboxdnac.cisco.com"
HEADERS = {"content-type":"application/json"}
USER = "devnetuser"
PASS = "Cisco123!"
AUTH = (USER, PASS) #  获得 token 时,带上用户名和密码的元祖
# 这是获取 token 的 URL
LOGIN_URL = DNA_URL + "/api/system/v1/auth/token"
# 先去获得一个 token
result = requests.post(url=LOGIN_URL, auth=AUTH, headers=HEADERS, verify=False)
# 真正拿到 token
TOKEN = result.json()['Token']
# 把 token 放在 headers 中
HEADERS['X-Auth-Token'] = TOKEN
# 这是获取资源的 URL
INVENTORY_URL = DNA_URL +"/dna/intent/api/v1/network-device"
response = requests.get(url=INVENTORY_URL, headers=HEADERS, verify=False)
response.ok
response.text
```

#### Session 验证
注意到这个 session 可以保留这个 cookie. 也就是说我们那这个 session 再去访问网站的其他资源时, 不需要再带上验证组了
```python
import requests
APIC_URL = "https://sandboxapicdc.cisco.com"
USER = "admin"
ASS = "ciscopsdt"
AUTH_BODY = {
     "aaaUser": {
         "attributes": {"name": USER, "pwd": PASS}
     }
}
LOGIN_URL = APIC_URL + "/api/aaaLogin.json"
session = requests.Session()
response = session.post(url=LOGIN_URL, json=AUTH_BODY, verify=False)
response.ok
response.cookies

# session 自己带上 cookie 访问, 不需要再放 AUTH_BODY 了
TENANT_URL = APIC_URL + "/api/node/class/fvTenant.json"
response = session.get(url=TENANT_URL, verify=False)
response.ok
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


# 关闭 http访问的告警
from urllib3.exceptions import InsecureRequestWarning  
from urllib3 import disable_warnings  
disable_warnings(InsecureRequestWarning)


AUTH = HTTPBasicAuth('cisco', 'cisco') # 输入验证
HEADERS = {'Accept': 'application/yang-data+json',
            'Content-Type': 'application/yang-data+json' }
            
URL = 'https://csr1v1/restconf/data/Cisco-IOS-XE-native:native'
# 用 get 方法
response = requests.get(URL, headers=HEADERS, auth=AUTH, verify=False)
# 返回值是 str, 转化为 json
result = json.loads(response.text) 
# 用 post 方法, payload 放在 data 参数中
# 注意: data 接受的是 string类型,所有要用 json.dumps 转化
esponse = requests.post(url, data=json.dumps(payload), headers=headers, auth=auth, verify=False)
```

# API Call
```python
import requests

url = "https://openapiv1.coinstats.app/coins"
# return json
headers = {
    "accept": "application/json",
    "X-API-KEY": "ZufUoOV21Q7FuKb9HLM/UfJ5EgRNjp0tTm7w6q3kO/4="
}

response = requests.get(url, headers=headers)

print(response.text)
```