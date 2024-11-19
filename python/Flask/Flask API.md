
创建一个 hello.py

```python
frmo flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
	return "<p>hello</p>"

```
运行 flask
```shell
# 会在当前目录寻找 app.py 文件
flask run
# --app hello 指定 Entry Point 为 hello.py 文件
flask --app hello run
```


requirements.txt
```
Flask-SQLAlchemy
psycopg2-binary # Flask-SQLAlchemy 使用的数据库引擎
```