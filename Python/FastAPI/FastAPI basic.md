### FastAPI和其他框架的对比
1. 原生支持async
2. 支持 documentation swagger

# FastAPI Components
FastAPI 由两个组件组成
- Fastapi 包
- Uvicorn 是一个服务器
使用pip 安装fastapi
```shell
pip install fastapi # 安装包
pip install uvicorn # 安装服务器
```

### 最基本的fastapi 应用
1. 构建入口的 `main.py` 文件
2. 初始化 FastAPI 对象 app
3. 用`@app.method(path)`定义路由, 并修饰返回的函数.
应用文件
``` python 
# main.py 文件
from fastapi import FastAPI

app = FastAPI()

# route
@app.get("/")
def home():
	return {"Data": "TEST"}
```
启动应用
```shell
# app是文件夹名, main 是 py 文件名 :app FastAPI 实例名
unicorn app.main:app
# 自动 reload
uvicorn app.main:app --reload
```

# Documentation
FastAPI 会自动使用swagger 生成docs
```
127.0.0.1/docs 
```
RAW json
```python
http://localhost:8000/openapi.json
```