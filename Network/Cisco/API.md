### HTTP Methods

- GET 参数在 URL 里是明文的
- POST 参数在 body 里, 用来创建对象
- PUT 更新整个对象, 替换但是不合并
- PATCH 只提供部分属性, 允许重复创建, 或者 merge
- DELETE 删除对象
### HTTP Response Code
- 1xx: Informational
- 2xx: Success
	- 200 get ok
	- 201 Post 创建成功
	- 204 patch ok 但是没有返回值, 表示 merge 成功
- 3xx: Redirection
- 4xx: Client Error
	- 400 Bad Request 请求语法错误, 可能是头部里 Content-Type 写错了
	- 404 页面不存在
	- 408 Request Timeout
	- 409 创建的对象已经存在, 不能重复创建
	- 429 被限流 Too Many Request
	- 413 Request Entity Too Large 请求包太大
- 5xx: Server Error
	- 500 Internal Server Error

### HTTP Authentication
- **Basic:** In this type of authentication, a Base64 encoder encodes username and password values.
- **Token-based:** In this type of authentication, the API server generates tokens (you can think of them as unique IDs) upon request and that token must accompany all future API requests. User and application tokens are possible and can often have timeouts; this decision is up to the API endpoint (server).
- **Session-based:** In this type of authentication, the API server generates a session cookie, which holds information necessary to make future API requests. Session cookies typically have an expiration time.

### Rate Limiting
限速器: 限制请求数量, 可以基于物理位置, 也可以基于 IP

### Payload Limiting
限制请求包的大小

Rate Limiting 算法
leaky bucket, fixed window, and Sliding Log.
### Reverse Proxy
反向代理
Proxy也称正向代理是有个人代替我去做事情. 比如 proxy server 是代替我去访问某个页面, 获取数据. 所以 Proxy 是中间人 middleman, 放在属于我的私网, 和我想要访问的 internet 之间, 起到屏障的作用. 
Reverse Proxy正好相反, 是从外网访问内网的服务器. 不应该让外部请求直接到服务器, 因为会被攻击. 所以要经过反向代理服务器 Ngnix. 同时也可以做 Load Balancing
总的来说, 正向代理是保护用户, 反向代理是保护服务器
API 接口可能会提供一个当前状态的查询方法

```python
# 减产 API 状态
def checkStatus(url)
	try: 
		# Check API status
		stats = requests.get(url + "/api/status")
		if status.status_code != '200'
			raise Exception("Unexpected status code: ", status.status_code)
	except Exception as e:
		print("API endpoint encountered an error" + e)
		
```


### Pagination
如果API 的返回值 next 是 null, 则说明到了最后一页
# Restful
restful 特征
- Client/server
- Stateless
- Uniform interface
### Restful 和 SQL 的对应关系

```sql
SELECT * 
FROM Users;
-- 等于 GET /users


SELECT * 
FROM Users
ORDER BY id
LIMIT 10 OFFSET 30;
-- 等于 GET /users?limit=10&offset=30

SELECT * 
FROM Users
WHERE id = '412';
-- 等于 GET /users?id=412

SELECT * 
FROM Users
WHERE age >= '50';
-- 等于 GET /users?age[gte]=50

SELECT * 
FROM Users
ORDER BY salary
ASC;
-- 等于 GET /users?sort=salary:asc
```

# RESTCONF
RESTCONF is a robust API that uses XML or JSON encoded data to represent data that YANG data models define.

Cisco Cloud Services Router 1000v (CSR 1000v) 支持 RestConf访问

RESTCONF 对 http 头部的编码要求是遵从 yang模型的 json
```shell
Accept: application/yang-data+json # 当接受数据时使用
Content-Type: application/yang-data+json  # 当发送 json 数据到远端时使用
```


# Webhooks
本质上是 API 相反的概念. API 是想服务器请求信息, Webhook 是服务器向 APP 发送信息. 
以支付系统为例, 当一个顾客完成了支付, 第三方支付系统, 会向购物网站 APP 发送 POST 请求, 请求中报了用户支付已经完成的信息. APP 收到这个请求后, 会触发发货的指令. 

