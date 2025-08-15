# 框架
python3 + django2.0
前端使用 html 5 websocket, 实现是 webssh
后台使用 paramiko 包来实现 ssh
当访问多台主机时, 要 python 的携程库gevnt,通过它来实现异步的通信 其中的并发的玩法, 可以是 thread, process
线程之间共享内容, 进程是有独立内存的

### gevent 
handle thousands of concurrent connections using a single thread. gevent 使用来实现异步通信

# 想法
- 把开发python 的环境容器化
- 把使用 mysql 数据库容器化, 并且替换为 postgre
- 怎么做到 docker 之间 python 和 mysql容器, postgre容器之间的通信
- 配置 mysql 连接方式的时候, 怎么把密码隐藏在配置或者环境文件中. 然后不上传的 repo
- 如果完成一个 CI/CD 的 pipeline, 在 docker 环境下开发, 但是可以自动部署到一个云端
# 环境
```shell
virtualenv -p /usr/local/bin/python3 --no-site-package python_env
pip install django==2.0.1
pip install pymysql==0.8.0

django-admin startproject ironfort
python manage.py runserver 0.0.0.0:8000
```

