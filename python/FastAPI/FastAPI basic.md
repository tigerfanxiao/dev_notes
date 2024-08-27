

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

#
@app.get("/")
def home():
	return {"Data": "TEST"}
```

run server
```shell
# working是文件名, app 是 FastAPI 实例名
uvicorn working:app --reload
```
查看自动生成的文档
```
127.0.0.1/docs 
```