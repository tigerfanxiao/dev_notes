
创建一个 `app.py`
- 默认的flask server running port 5000
```python
frmo flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
	return "<p>hello</p>"

```
run flask app
```shell
# 默认情况下flask会在当前目录寻找 app.py 文件
flask run
# --app hello 指定 Entry Point 为 hello.py 文件
flask --app hello run
# 默认情况下 flask 的端口是5000
```

### Flask 的依赖库
`requirements.txt`
```
Flask-SQLAlchemy
psycopg2-binary # Flask-SQLAlchemy 使用的数据库引擎
```