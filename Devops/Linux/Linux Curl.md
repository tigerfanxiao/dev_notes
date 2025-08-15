# 测试HTTP服务
### get
```shell
curl -X 'GET' \  # method
	'http://127.0.0.1:8000/' \ # base url
	-H 'accept: application/json' # header
```

### post
```shell
curl -X 'POST' \ 
'http://127.0.0.1:8000/create-item/3' \ 
-H 'accept: application/json' \ 
-H 'Content-Type: application/json' \ 
-d '{ "name": "xiao", "price": 10, "brand": "fan" }'
```




