对比 Flask
1. 原生支持async
2. 支持 documentation swagger

# Installation
```shell
pip install fastapi
pip install uvicorn
```

# Initiate Project
1. 构建入口的 py 文件
2. 初始化 FastAPI 对象 app
3. 用`@app.method(path)`定义路由, 并修饰返回的函数.

``` python 
# working.py 文件
from fastapi import FastAPI

app = FastAPI()

# route
@app.get("/")
def home():
	return {"Data": "TEST"}
```

run server
```shell
# app是文件夹名, main 是 py 文件名 :app FastAPI 实例名
unicorn app.main:app
# 自动 reload
uvicorn app.main:app --reload
```
查看自动生成的API文档
```
127.0.0.1/docs 
```
RAW json
```python
http://localhost:8000/openapi.json
```