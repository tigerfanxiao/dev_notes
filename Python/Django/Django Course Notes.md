
目标
0. 整个项目做三遍
1. 跟着视频完整做一遍
2. 不看视频, 根据自己的笔记能够完整做一遍
3. 精简文档, 快速开发做一遍. 看看这个项目有没有扩展的地方. 变体或者和其他项目打通

API Feature
- user authentication, Django有自己的 authentication 模块, 但是对于 flask 的话, 就需要自己找组件了
- creating object
- Filtering & sorting objects
- uploading & viewing images

django 3.2 
项目中会涉及平时运维中的, 升级 api 的工作, 这部分是 devops 的

# Dev
### unit test

> [!key]
使用 docker 做一次性的用例测试 
```shell
	dockercompose run --rm app sh -c "python manage.py test"
```
- docker-compose run app:
    Runs a one-off container from the service named app defined in your docker-compose.yml. This is **not** the same as docker-compose up; it just runs a command and exits.
- sh -c "python manage.py test":
    Tells the container to start a shell (sh) and run the command python manage.py test inside it. This command runs Django’s test suite.


为啥现在要用REST 因为, 手机终端和其他客户端都可以使用同一个后端
19 endpoints
user authentication
django restframework 是 django 的 addon
需要学习好 postgres

### 项目目录结构
```
app/ # Django project
app/core/ # Code shared between multiple apps
app/user # User related code
app/recipe # Receip related code
```
构建 requirements.txt
```
Django>=3.2.4,<3.3
djangorestframework>=3.12.4,<3.13
```
### TDD
TDD is software development practice. 特点是先写根据 feature 写测试用例, 在写开发代码
django test suite 是 django 自带的 unit test 模块, 
Django 还增加一些新的特性
- Test Client - dummy web browser
- Simulate authentication
- Temporary database. 每次执行 test 都会创建, 测试完成后会clear. 这个行为是可以修改的
Django Rest framework 增加的测试特性
- API test client
测试文件的放置位置
- 在每个 app 下面放置 `tests.py`
- 创建 tests/ 子目录
	- test module starts with `test_`
	- test directories mush contain `__init__.py`
- 注意: 两种方式只能用一种, 否则会造成 ImportError


Test Classes
- `SimpleTestCase`
	- No database integration
	- Useful if no database is required for your test
	- Save time executing tests
- TestCase
	- have database

```python
"""
下面只是单纯的单例测试, 没有 mock
"""
from django.test import SimpleTestCase

from app_two import views # 引入需要测试的 object

# 创建 Test Class 
class ViewTests(SimpleTestCase): 
	# test method start with test_
	# help function no need to put test_ 
	def test_make_list_unique(self):
		sample_items = [1, 1, 2, 2, 3, 4, 5, 5]
		res = views.remove_duplicates(sample_items)
		self.assertEqual(res, [1, 2, 3, 4, 5])
```


使用 docker compose 命令来执行 unit test
```shell
docker compose run --rm app sh -c "python manage.py test"
```

Mocking
- 在测试中, 比如用户注册, 其中有个动作是需要发送邮件的. 但是我不想每次测试都测试这一步邮件发送. 就用 mocking 把这步跳过去
- 加快测试的速度. 比如我们需要写一个程序来不断的测试某个服务是否已经起来的了. 会用到 sleep 函数, 但是在实际测试中, 不需要真的运行 sleep 函数, 用 mock 函数来替代
how to mock code - use `unittest.mock`
- MagicMock/Mock - Replace read object
- pathch - Override code for tests

testing web request
django-restframework 提供了 APIClient 专门用来测试 API
```python
from django.test import SimpleTest
from rest_framework.test import APIClient

class TestViews(SimpleTestCase):
	def test_get_greeting(self):
		client = APIClient()
		res = client.get('/greetings/')

		self.assertEqual(res.status_code, 200)
		self.assertEqual(
			res.data, 
			["Hello", "Hola"]
		)

```
### Docker
从 docker 20.10 开始 docker-compose 命令集成到了 docker 中, 所以所有的 docker-compose 命令可以用 docker compose 命令来替代

dockerfile 和 docker-compose 的区别
- dockerfile: lists of steps for create image
- docker-compose how our docker images should be used, define our service
	- name
	- port mappings
	- volume mappings
- 所以当我们是使用docker 镜像来运行容器的时候是使用 docker compose 命令的
- docker hub 是用来 pull image 的, 但是有 rate limit
	- 100 pulls /6hr for unauthenticated users
	- 200 pull /6hr for authenticated user
- github action 则是是个 shared service
	- 100 pull /6hr 是全球所有开发者共享的

在 docker hub 上生成这个项目使用的 access token, 这个 token 应该是用户, 我们本地构建镜像后, 上传到 docker hub上, 然后 github action 可以是用这个 token 去访问 docker hub 拉取镜像
我们到 github 的 setting 中的 secret 中把 token上传上去
add new repository token
这里创建两个环境变量
- DOCKER_USER
- DOCKER_TOKEN

```shell
docker compose build 
docker build 
# 两个命令都会新建 image, 在开发的时候, 只要用 docker compose 的命令
```

`.dockerignore` 文件是用来exclude 在某些文件目录下去寻找 Dockerfile或者 docker-compose 文件

docker-compose
- app和 db 是并列创建的两个服务. Docker-compose 会默认给两个服务创建一个可以通信的网络, 通过服务的名字来访问. db 就是数据的 hostname
- 我们使用 volume 来存放数据库的数据, 即持久化的数据
```yaml
services: 
	app:
		depends_on: # 注意这里只保证 service is on, 但是不保证 application is running
			- db
		environment:
			- DB_HOST=db # 这里定义了服务 app 怎么连接服务 db, hostname 就是 db
			- DB_NAME=devdb # 和下面 db 中定义的环境变量一致
			- DB_USER=devuse 
			- DB_PASSWORD=changeme 
	db:
		image: postgres:13-alpine
		volumes: 
			- dev-db-data: /var/lib/postgresql/data
		environment:
			- POSTGRES_DB=devdb
			- POSTGRES_USER=devuser
			- POSTGRES_PASSWORD=changeme
volumes:
	dev-db-data: # docker-compose 会自己去找一个本地的目录去存放名字为 dev-db-data的数据

```

docker compose 命令
```shell
# 如果要运行容器不关闭
docker compose up
# ctrl + c 之后, 只是停止 container 的运行
# 彻底清楚这些 container
docker compose down 
# 如果修改了 Dockerfile 和 requirements.txt 意味着有新的依赖加入, 就需要重新构造镜像
docker compose build
```
=======
`.dockerignore` 文件是用来exclude 在某些文件目录下去寻找 Dockerfile或者 docker-compose 文件 
login with docker hub from terminal
```shell

```
### Github action
Github action 类似于 Travis-CI, Giblab CI/CD, Jenkins. 功能有
- Deployment
- Code linting
- Unit tests
github 有 3 部分组成
- trigger(push to github)
- jub like run unit test
- results, Success/fail
Pricing
- Charged per minutes
- 2000 free minute
- 给全球用户使用的 shared IP address

```

```


>>>>>>> 39440c9 (add notes)
### git
构建 github repo recipe-app-api
 一般先在 github 上创建 repo, github 上提供了, 基于编程语言个 gitignore 文件和不同的 License 选择

# Linting
- Linting is tool to check code formatting
- highlights error, typos, formatting issues
install flake8 package
run it through docker compose
解决 linting 问题的时候, 建议从下至上的顺序来修改. 因为改动 linting 中的空行, 很可能会影响别的 linting 问题的定位行数
使用 docker compose 命令来执行 linting 命令
```shell
docker compose run -rm app sh -c "flake8"
```
因为 linting 是开发使用的, 所以只有在 requirement.dev.txt 中才需要安装
flake是需要configuration 文件来exclude 不需要分析 linting 的目录的
flake8 的配置文件, 是放在 manage.py 并行的目录下, 这个是项目的根目录

```
[flake8]
exclude = 
    migrations,
    __pycache__,
    manage.py,
    settings.py
```

# Django

创建 django 项目 app
- 创建项目时, 会先创建一个 app 的项目目录, 在 app 的项目目录下会在创建一个 app 的应用目录
- 注意: 虽然这里是 --rm 会删除 container, 但是因为 volume 打通了, 创建的文件会留下来

```shell
docker compose run --rm app sh -c "django-admin startproject app ."
```

如果要构造一个 core app,需要使用命令. 注意: 和创建项目时使用的命令完全不同 (可以查询 django 文档)
```shell
docker compose run --rm app sh -c "python manage.py startapp core "
```

```python
INSTALLED_APPS = [
	"django.contrib.admin",
	"django.contrib.auth",
	"django.contrib.contenttypes",
	"django.contrib.sessions",
	"django.contrib.messages",
	"django.contrib.staticfiles",
	"core", # 除了一开始初始项目的app之外, 后面添加的 app 都要在 setting 中增加
]
```
### 构建自己的Django custom management command
参考 django 文档

```
myapp/
├── management/
│   ├── __init__.py
│   └── commands/
│       ├── __init__.py
│       └── wait_for_db.py
```

```python
from django.core.management.base import BaseCommand
from django.db.utils import OperationalError
import psycopg2

class Command(BaseCommand):
    """Django command to wait for database"""
	def handle(self, *args, **options):
		"""Handle the command"""
		self.stdout.write("Waiting for database...")
		import time
		db_up = False
		while db_up is False:
			try:
				self.check(databases=["default"])
				db_up = True
			except (psycopg2.OperationalError, OperationalError):
				self.stdout.write("Database unavailable, waiting 1 second...")
				time.sleep(1) 
		self.stdout.write(self.style.SUCCESS("Database available!"))
```
 在 Dockerfile 中添加
```yaml
CMD ["sh", "-c", "python manage.py wait_for_db && python manage.py migrate && gunicorn myproject.wsgi:application"]
```
# Database Postgres

Python package: Psycopg2 is most popular PostgreSQL adaptor for python
django 连接数据库, 这些信息是定义在 setting.py中的
- Engine (type of database)
- Host(IP or domain name for database)
- Port (5432 for postgres)
- Database name
- username
- password

为了解决数据库服务起来, 但是 posgresql 没有起来, 造成 django 崩溃的问题. 我们要创建一个 core app 来轮询查询数据库是否connection

# 其他想法

### 后续学习
DevOps Deployment Automation with Terraform, AWS and Docker 同一个老师的 devops 课程, 部署后端代码 25 个小时


### Pandas
复习之前在 kaggle 上考的 pandas 证书
https://www.kaggle.com/learn/pandas
[[Python Pandas]]
### SQL 即其他免费的学习资源
学习在 kaggle 上的 sql 课程并获得证书
https://www.kaggle.com/learn

