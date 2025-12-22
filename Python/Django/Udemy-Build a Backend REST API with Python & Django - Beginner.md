course [Build a Backend REST API with Python & Django - Beginner](https://www.udemy.com/course/django-python/)
[code](https://github.com/LondonAppDev/profiles-rest-api)
# Thoughts
技术本质上是为了商业服务的, 至少目前我的这个情况下. 我们这是用代码去实现商业的价值. 从这个意义上看, 我们是来解决问题的. 要解决问题就要看需要实现的目标和成本. 在目标和成本有一个比较清晰的认识之后, 我们才能选定技术方案

如果要做CIO, 需要会做预算, 每年在IT成本上的投入和整体的企业营收之间的关系. 如果向CTO去解释, 我们需要多少经费来实现怎样的目的

从跟目前这个项目看开发和部署确实是两种完全不同的活, 都要花非常多的时间. 所以需要用CICD, 把部署自动化, 这样就可以把时间专注在开发上了

### How to study
1. 需要把不同的模块分开, 先专注于Django Rest Framework本身. 而不是环境部署上. 因为DRF是主要矛盾. 主要矛盾的逻辑高通了, 再逐个去研究分支的问题
2. 观察DRF的组件, 应该其他语言, 想nodejs应该是有共性的
3. 久坐和锻炼身体
4. 问自己问题, 解答自己的问题. 然后是假设要去叫别人怎么开发Django Rest Framework API
### Questions
- supervisorctl 在linux 中是什么工具?
- 当前vagrant和EC2的ubuntu版本不一致, 后期可以更改
- python的生产环境的服务器uwsgi, 还是ngnix, 这是什么关系, 生产中真的用吗
- 能用Firebase和DRF合作吗. 本质是Firebase是一个基本能取代django rest framework的东西, 只要前端就好了
- 怎么和Jenkins联系起来
### Move on
- Development
	- 这个项目没有写怎么开发Log, 记录程序的一些动作, 比如用户登录了, 用户的行为, 用于排错
	- 这个项目没有Exception
	- 这个项目没有Unit Test
	- 这个项目没有消息对列的内容
	- 这个项目没有异步请求的内容
	- 这个项目没有文档
- 学习使用vagrant环境去开发和部署
	- 学习了API开发中权限管理的部分. 这是36K工资的工作
	- 进阶的advance课程
- Deployment
	- 在AWS上基本EC2
	- 需要研究在EC2上具体的deploy方法, 这是手工deploy的能力uwsgi得研究一下, 理论上说安装了就会自动关联整个服务器, 但是nginx怎么配置呢?
	- 相同的老师有专门的课程
	- AWS Solution Architect Associate 实操课程

### Tech-Stack
36K+ Mid-level Python Backend
- Dev
	- Python Syntax
		- Comprehension list, 正则
	- Python Package
	- Python Framework
		- Django Rest Framework 3.9.2
		- Django 2.2
		- UnitTest 框架
- Database
	- Sqlite
	- Postgres
- Devops
	- Basic Linux
	- Git/Github 
	- Reverse Proxy - Nginx
- Cloud Computing
		- AWS
			- Solution Architect Associate Certificate 获得
- Project

48K+ Data Engineer + DevOps
	- FastAPI 框架
	- Docker 开发环境
	- Linux
		- Coursera 上的 Linux 课程
		- Bash Script
	- Database
		- DynamoDB
		- Redis
		- Memcache
	- Devops
		- Containerization
			- Docker
		- Nginx
		- CICD
		- Iac
			- Terraform
			- Ansible
	- Cloud Computing
		- AWS 
			- Coursera上的 Data Engineer 课程
			- AWS Data Engineer Associate Certificate 获得
		- Azure
		- GCG
	- Project Management
		- PMP
65K+
	- K8s 认证
		- CKA 获得
			- 如何时间Canary 部署
			- 如何时间HPA 弹性
		- CKS 计划在25年年底
		- CKAD 计划在25年年底
	- Linux 熟练掌握, 或者认证
	- AWS 认证
		- Solution Architect Professional Certificate 获得
		- Devops Professional Certificate 计划在26年年初
		- Nework Speciality Certificate 计划在26年年初
		- Security Speciality Certificate 计划在26年年初
85K+ Fullstack + Devops
	- Frontend
		- HTML
		- CSS
		- JS
		- React
		

# 环境
- 在windows上安装virtual box, vagrant(是命令行的)
### Vagrant
```shell
vagrant --version # verify

# 从repo上下载vagrantfile
# 一下是初始化的命令
vagrant init ubuntu/bionic64

vagrant up # 启动或重启虚拟机
vagrant ssh # 进入虚拟机
vagrant global-status # 查看所有虚拟机
vagrant halt # 终止vm

# 所有的/vagrant内创建的文件都和Vagrantfile所在的文件目录同步
cd /vagrant # 进入host和虚拟机的共享目录
```
通过修改 Vagrantfile 来变更环境
```shell
config.vm.box = "ubuntu/bionic64" 
# 切换到
config.vm.box = "ubuntu/jammy64"
```
# Django
Django和Django Restful 的关系是什么. 其实Django和Django Restful是不同的包. 因为在开发的过程中, 我们可以看到有些包使用Django中直接import的, 有些包是从Rest Framework中import的, 注意区分
### Develop environment
1. 使用venv模块创建python虚拟环境
2. 通过requirements.txt 安装
```shell
# create env
python -m venv ~/env # 这里制定的目录不是和vagrant同步的目录, 所以环境只是和vagrant实例中
# activate env
source ~/env/bin/activate
# 退出虚拟环境
deactivate
```
### Framework
```shell
# requirements.txt
django==2.2 # django framework 2019
djangorestframework==3.9.2 # drf framework 2019
```

通过`requirements.txt` 安装虚拟环境的包
```shell
# 在虚拟环境被激活的情况下
pip install -r requirements.txt 
```
### django admin
```shell
# 创建项目目录和 manage.py 文件
django-admin.py startproject profileproject .
# 创建一个应用
python manage.py startapp profiles_api

# 运行 django server
python manage.py runserver 0.0.0.0:8000

# 创建sql
python manage.py migrate
# create super user, 这个命令在运行 migrate之前是不能运行的. 因为sqlite中还没有用户列表
python manage.py createsuperuser
```

### Create Project and App
区分Project和App两个概念
- 项目是一个大的集合, 在整个大的集合项目不同的功能模块都是App. 一个项目至少包含一个App, 所以创建完项目后就要创建App
- 创建project时候, 就会在本地生成project的目录和`manage.py`文件
- 创建项目和创建App是两个不同的工具
- 创建项目是使用`django-admin.py`, 创建app是使用`python manage.py` 
- 在`settings.py`中注册rest_framework 和 app
```shell
# 执行一下命令后, 会在本地创建一个 my_project 文件夹, 里面有settings.py 和 urls.py
django-admin.py startproject my_project .

# 创建app, 使用 manage.py
python manage.py startapp my_api

# 启动django rest_framework
python manage.py runserver 0.0.0.0:8000

# 浏览器访问
127.0.0.1:8000
127.0.0.1:8000/admin

# create super user
python manage.py createsuperuser
```
在`settings.py`中注册增加三行, 注册rest_framework 和 app
注意: 这里面的顺序也是有讲究的
```python
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework', # 添加 rest framework
    'rest_framework.authtoken', # 添加 认证方式
    'profiles_api', # 添加自定义的 app
]
```
这个 rest_framework.authtoken 添加只有, 在admin里面就会多下面这个东西
![[Pasted image 20251222192356.png]]

## Servers
服务器分为Application Server和Web-server两种
	- Application server: Django, Flask, Spring Boot, 用来处理业务逻辑, 和数据库沟通
	- Web server: Nginx, Apache, 用来处理http请求, 反馈静态内容
	- Application Server 往往部署在 Web-server的后面
- 在python中, Application Server 有wsgi和asgi 两种, 即asgi可以处理异步请求, wsgi不行
- wsgi和asgi都是Application Server的接口规范, 不是产品本身
- 市场上成熟的wsgi服务器有 uWSGI, Gunicorn ASGI 服务器有 Uvicorn
- wsgi和asgi都提供python的运行环境, 就是说, python通过接收到达这些服务器的request, 来处理和响应
- 无论是wsgi还是asgi 其实都是开发中使用的服务器. 真正在生产环境中, 其实还有一种服务器类型叫 web-server, 这种服务器本身用来承接海量的需求. 将承接到的需求是转交给wsgi/asgi服务器来处理. 如果需求只是获取静态文件, 那么这种需求就可以直接到static文件的存储中提取文件反馈给用户, 不需要进过python wsgi和asgi 服务器来处理. 这也就解释了 `python manage.py collectstatic --noinput` 的作用. 在实际生产环境部署中, ngnix需要知道静态内容的存储路径的. 这些内容应该在nginx的配置文件中写明

### Nginx
学习方法: 在学习完django开发后, 学习部署时才专门学习

在云的环境中, 比如AWS本身是提供ALB或者NLB的, 在这种情况下, 是否还需要再VPC内部署Nginx呢, 答案是肯定的. 原因是这样的部署方式, 更安全, 因为ALB或者NLB都是public network, 而vpc是private network. 而Nginx有颗粒度更细的路由分发能力, 可以依据7层, header, cookie, session data 分发策略.
以下是在VPC中还需要部署Nginx的理由
1. 用户的session persistence 需要在内网做
2. SSL termination 也需要放在VPC里做
3. 流量在一个VPC中不同的微服务之间的分发需要在内网做
在K8s的语境中, Ingress Controller 就是k8s的Reverse Proxy组件
在nodejs环境里, expressJS是一个轻量级的Reverse Proxy
那同是Reverse Proxy的Nginx和expressJS他们区别在哪里?
- Nginx load balance 在不同的服务器之间进行, expressJS则是在一个服务器上的不同CPU内核之间
- Nginx 一般称为Web-server, expressJS则称为App Server, 也称为web-framework
- Nginx非常高效的处理静态文件, 压缩文件等. expressJS则是用来处理业务逻辑

所以在生产环境中, 往往是多个expressJS服务器前面放了一个nginx
![[Pasted image 20250808170912.png]]

Nginx 是一种 Reverse-Proxy 也成为web-server, 即反向代理. 作用是直接承接互联网上用户的请求, 然后将这些请求进行分类, 负载分担等操作, 发送给web-server.
Nginx 的作用
- serve 静态文件(能力非常强大), gzip压缩
- Reverse Proxy, send request to application server
- handles SSL/TLS termination, caching, load balancing
```shell
server {
    listen 80 default_server;

    location /static {
        alias /usr/local/apps/profiles-rest-api/static;
    }

    location / {
        proxy_pass        http://127.0.0.1:9000/;
        proxy_set_header  Host                $host;
        proxy_set_header  X-Real-IP           $remote_addr;
        proxy_set_header  X-Forwarded-For     $remote_addr;
        proxy_set_header  X-Forwarded-Proto   $scheme;
        proxy_redirect    off;
    }
}
```
### Django admin interface

需要先创建superuser
```
http://127.0.0.0:8000/admin
```
使用superuser账户登录

### `ViewSet`
我们通过Viewset来定义Restful的动作. 包括有 create, reading, update这些
定义Viewset的方法

```python

```
### URL

### Permissions

### Model
model 可以认为是一种数据模型. 在数据库中是以一张表的形式出现. 如果我们在Django中定义了一个model模型, 那么在Django的数据库中就会定一张表

定义模型的方式
1. 在App的目录下找到 `models.py`文件
2. 在`models.py`中定义模型
3. 定义models中的字段, 是否有`models.ForeignKey()`
4. 定义模型实例的字符串表述方式 `__str__()`
5. 使用`makemigrations` 构造数据库的表
6. 把model注册到 Django admin, 就能在Django admin中看到
7. 定义model的Serializer
8. 为model创建ViewSet
9. 为Viewset创建URL
```python
from django.db import models

class MyModel(models.Model):
	"""What this model is"""
	# 定义model的字段
	field1 = models.CharField(max_length=255) # 字符串字段
	field2 = models.DateTimeField(auto_now_add=True) # 时间戳

	def __str__(self):
		return field1
	
```

把模型注册到Django admin中
```python
from django.contrib import admin
from profile_api import models # 从自定义的app中引入models.py

# 注册模型
admin.site.register(models.ProfileFeedItem) # 自定义的模型
```
在注册完成后, 可以在admin中对应的app下, 看到model的名字
![[Pasted image 20250802203527.png]]
为模型在数据库中构造表的命令
```shell
python manage.py makemigrations # 生成构造表的方案
'''
profiles_api/migrations/0002_profilefeeditem.py
    - Create model ProfileFeedItem
'''
python manage.py migrate # 链接数据库构造表
'''
Operations to perform:
  Apply all migrations: admin, auth, authtoken, contenttypes, profiles_api, sessions
Running migrations:
  Applying profiles_api.0002_profilefeeditem... OK
'''
```

### `serializer`
当我们为模型构建实例的时候, 用户的输入是需要检验的, 比如姓名不能超过255个字符. 我们用Serializer来做数据合规的校验. 
serializer 定义的方式
1. 在app的目录下创建 `serializier.py`的文件
2. 构造一个Serializer类
3. 在Serializer里面创建一个Meta类. 
	1. model 对那个模型进行校验
	2. fields 有哪些字段需要检验
	3. extra_kwargs 那个字段有特殊的需求, 比如read_only

```python
from rest_framework import serializers

from profiles_api import models


class ProfileFeedItemSerializer(serializers.ModelSerializer)
	"""Serializer profile feed items"""
	class Meta:
		model = models.ProfileFeedItem
		fields = ('id', 'user_profile', 'status_text', 'created_on')
		extra_kwargs = {'user_profile': {'read_only': True}}
```

### Foreign Key
模型和模型之间的关系, 有一对一, 一对多, 多对多三种. 这三种关系都通过外键来建立.
举个例子: 用户和帖子的关系. 一个用户可以发多个帖子. 那么在定义帖子model的时候, 都要有一个字段说明是那个用户发的. 那整个用户ID对于帖子model来说就是一个外键. 
设想如果我们删除了用户, 那用户的帖子就需要一起删除. 这种主键和外键的关系就是cascade
定义外键的方式
1. 在定义model的时候, 确定那个字段是关联别的model的字段, 定义为外键
2. 指定如果对方被删除后, 自己是否会删除相关数据
```python
from django.db import models

class ProfileFeedItem(models.Model):
	user_profile = models.ForeignKey(
		settings.AUTH_USER_MODEL, # 指定关联的其他模型
		on_delete=models.CASCADE # 指定删除方式
	)
```

### static

```python
# settings.py
STATIC_ROOT = 'static/'
```
### Django Others
如果要在程序里调用, 项目目录下的`settings.py` 文件中的内容
```python
from django.conf import settings
```


## AWS
### 构建EC2实例
1. 配置本地电脑ssh到实例
2. 创建EC2实例的步骤
	1. 目前最低的Ubuntu 版本是22.04
3. 把deploy的执行文件上传到github
4. 通过curl将setup.sh 下载到实例上
在EC2 上传keypair 用于笔记本电脑ssh访问EC2的实例
import keypair
```shell
cat ~/.ssh/id_ed.pub
```
配置key pair
![[Pasted image 20250802231948.png]]

配置security group 允许ssh和http访问
注意: 选择auto-assign public IP
![[Pasted image 20250802231828.png]]
远程登录EC2 实例
```shell
# 默认的实例的有用户ubuntu
ssh ubuntu@dns_address
```
直接从github上下载setup.sh脚本文件. 通过raw 文本找到文件url
```shell
# -s sient mode 
# -L following redirect 
# bash - means read from stdin  
curl -sL https://raw.githubusercontent.com/tigerfanxiao/profiles-rest-api/refs/heads/main/deploy/setup.sh | sudo bash -
```
访问, 注意需要指定是http, 而不是https
```shell
http://ec2-54-225-44-73.compute-1.amazonaws.com/api/
```
### supervisor
https://www.youtube.com/watch?v=KPCSh79GCCE
上面这个视频是一个简单的supervisor教程. 简单的来说, supervisor是一个linux上的应用, 这个应用的作用是管理Linux上运行的进程. 比如说Django server进程, 如果用run server 在生产环境上跑起来之后, 如果出现了server中断, supervisor可以自动重启这个进程. 我们也可以通过supervisor来关闭和启动django进程. 同事supervisor还有GUI界面, 通过9001端口可以访问. 
```shell
environment = 
	DEBUG=0 # 给系统这是环境变量
```

Socket 是一种双向通信机制. 常见的socket有
- TCP Socket, 建立一种有链接的, 稳定的双向通信机制
- UDP Socket, 建立一种无连接的, 不保障包顺序的双向通信
- Unix domain socket
Unix domain socket also named as IPC socket, Inter-process Communication Socket用户unix系统上不同进程间的双向通信. 现在还流行RPC Remote Procedure Call, 即实现两台不同物理机上的进程互相通信
# Others
HTTP Status
200 正确
400 页面不催在
401 Authentication error
302 页面跳转

# 整体编程过程

8/12

我想要自己构建一个项目

- 这个项目的最终目的是学会自己写 django rest_framework

- 这个项目可以是用 docker 的, 或者直接在本地虚拟环境运行. 但是开发环境不是这个项目的最重要的目的.

- 这个项目最基本的需要亲手实现 drf 框架和课程中的讲到的所有功能


# UV

为什么要学 UV?
- UV 一个工具可以同时实现 python 包管理和 python 版本管理两个作用
- 第二 UV 比 PIP 快 10 倍到 100 倍, 可以快速构建自己的环境
- install uv
```shell
# on mac
brew install uv
# on others
pip install uv
```

Q: how to use docker with uv
use uv to install python

```shell
# show all available python version online
uv python list
uv python list --only-installed # show all localled installed python list


# install python version 3.8
uv python install 3.8

# find the specific python version on the local host
uv python find 3.8

# uninstall specific python version
uv python uninstall 3.9
```

use uv to run python
```shell

# run python script with specific python version

uv run --python 3.9.21 main.py

# run python script with specific package or module

uv run --with rich --with requests --python 3.8 main.py

  

# generate a script at the beginning of main.py
uv init --script main.py --python 3.9.21

# add package to the script
uv add --script main.py "rich"

# now you can run main.py directly
uv run main.py
```

use uv for project

```shell
# 在本地目录下先创建项目文件夹后, 再在项目文件夹里面创建项目文件
uv init my_project

# install python package, and create .venv folder
uv add django==2.2
source .venv/bin/activate

# remove dependency

uv remove requests

  

# 如果修改了 pyproject.toml中的依赖, 可以用下面的命令, 将 uv 的配置同步到.venv中

uv sync

# generate uv lock file manually

uv lock

```

  

# 初始话项目

激活本地的虚拟环境

```shell

source .venv/bin/activate

# 使用 django-admin 命令创建项目

django-admin.py startproject profilesproject .

# 创建 app

python manage.py startapp profiles_api

# 运行 django 服务器

python manage.py runserver 0.0.0.0:8000

  

```

  

# 如果 django 想要使用定制的 user, 比如使用 email 作为用户名

首先在 profiles_api 中修改model.py

```python
from django.contrib.auth.models import AbstractBaseUser
from django.contrib.auth.models import PermissionsMixin
from django.contrib.auth.models import BaseUserManager

  

# 下面这个类是必须要写的, 因为如果修改了 djang 的默认用户类, python manage.py createsuperuser 执行的时候, 还是会使用默认的类. 新建一个 UserManager 的类
class UserProfileManager(BaseUserManager):
	"""Manager for user profiles"""
	# django-cli 用来穿管用户的方法
	def create_user(self, email, name, password=None):
		"""Create a new user profile"""
		if not email:
			raise ValueError('User must have an email address')
		email = self.normalize_email(email)
		user = self.model(email=email, name=name)
		user.set_password(password)
		user.save(using=self._db)
		return user

	def create_superuser(self, email, name, password=None):
		"""Create a new super with given details"""
		user = self.create_user(email, name, password)
		user.is_superuser = True
		user.is_staff = True
		user.save(using=self._db)
		return user
	
class UserProfile(AbstractBaseUser, PermissionsMixin):
	"""Database model for users in the system"""
	email = models.EmailField(max_length=255, unique=True)
	name = models.CharField(max_length=255)
	is_active = models.BooleanField(default=True)
	is_staff = models.BooleanField(default=False)
	# 因为django cli默认是和默认的用户模型打交道的
	# 我们自己定了用户模型后, 就需要告诉django-cli怎么和我们的模型打交道
	objects = UserProfileManager()
	USERNAME_FIELD = "email" # 我们用 email 字段替换了默认的username字段
	REQUIRED_FIELDS = ["name"] # 制定了哪些字段是必须字段

	def get_full_name(self):
		"""Retrieve full name of user"""
		return self.name
	
	def get_short_name(self):
		"""Retrieve short name of user"""
		return self.name
	
	def __str__(self):
		"""Return string reprensation of user"""
		return self.email
```

  

因为使用了定制的 user 模型, 所以在创建 superuser 管理员账户之前, 需要先修改模型
在创建完上面自定义的 usermodel 之后, 需要新使用 migrate 命令, 对和模型有关的数据的表结构进行更改.

需要把这个 model 注册到 admin 中, 否则登录django 后台无法查看

```shell

python manage.py makemigrations profiles_api # 需要指定那个 app, 创建修改数据库的计划

python manage.py migrates # 实际执行修改数据库的动作

  

# 完成上面两步操作后, 创建超级账户, 这个账户在开发环境中是存在本地的sqlite数据库里的

python manage.py createsuperuser

```

创建超级用户 tigerfanxiao@gmail.com/Fx926926

可以通过 127.0.0.1:8000/admin 登录来访问 django 后台, 可以看到 userprofile 模型中的 tigerfanxiao@gmail.com 用户

  

在 django rest framework 中有两种方法来创建 API endpoint. APIView 和 ViewSet

注意, 这两个类其实是 DRF 中的, 和 Django 本身的 view 不同. 这两个类默认就支持 HTTP 的那些 REST 请求(get, post, put, delete, etc.).

创建了 API View 之后, 我们还要创建 router, 运行用户通过指定的 URL 来调用某个 View 函数

此外对于用户的输入的内容, 我们还需要创建 Serializer 来处理. 所以View, route, Serializer 三个模块一起构成了一个 API Endpoint

restful 的几个方式都在 apiview 里面去实现

APIViews 作用

- Perfect for implementing complex logic

- Calling other APIs

- Working for local files

  

如果定义一个 APIView

1. 定义一个类继承 rest_framework中的 APIView的类

2. 重写 get, post, put, patch, delete 方法, 这写方法里面都有 request 作为参数, 返回值都是 Response 对象

3. 在APIView 中定义serializer_class, 在 post 方法中, 就需要用客户的输入, 创建一个 serializer 对象

  
  

以下是 APIView 的一个例子, 例子里api 调用没有涉及数据库的访问, 只是通过用户提交的数据, 返回

```python

from rest_framework.views import APIView

from rest_framework.response import Response

  
  

# 创建一个类, 继承 APIView

class HelloApiView(APIView):

"""Test API View"""

# 这里是直接传一个 serializer 类进去

serializer_class = serializers.HelloSerializer

  

def get(self, request, format=None):

"""Returns a list of APIView features"""

an_apiview = [

'Uses HTTP methods as function (get, post, patch, put, delete)',

'Is similar to a traditional Django View',

'Gives you the most control over you application logic',

'Is mapped manually to URLs',

]

  

# Response 里面都是放字典的

return Response({'message': 'Hello!', 'an_apiview': an_apiview})

  

def post(self, request):

"""Create a hello message with our name"""

# 注意这里self.searializzer_class 是调用了在类里定义的 serializer_class

# 这里通过用户的输入, 创建了一个 serializer 对象

serializer = self.serializer_class(data=request.data)

  

if serializer.is_valid(): # serializer 的校验方法

name = serializer.validated_data.get('name') # 校验好的数据中取到 name 的值

message = f'Hello {name}'

return Response({'message': message})

else:

return Response(

serializer.errors, # 如果 Serializer 失败, 返回的是serializer.errors 表示验证失败

status=status.HTTP_400_BAD_REQUEST

)

  

# 注意到下面的 put, patch, delete 代码都是需要 pk 作为参数的

def put(self, request, pk=None):

"""Handle updating an object"""

return Response({'method':'PUT'})

  

def patch(self, request, pk=None):

"""Handle a partical update of an object"""

return Response({'method', "PATCH"})

  

def delete(self, request, pk=None):

"""Delete an object"""

return Response({'method', "DELETE"})

  

```

  

下面定义了 APIView 的 Serializer

  

```python

from rest_framework import serializers

from profiles_api import models

  
  

class HelloSerializer(serializers.Serializer):

"""Serializes a name field for testing our APIView"""

# 这里定义了, 用户的输入, request 中的有一个键值对 name, 用户输入的name, 不能长于 10 个字符

# 注意, 这里的用的是serializers.CharField, 并不是 models.CharField 可以查看 Serialzier 的行为是校验

# 这里其实定义了用户的输入, 在APIView 的页面上可以看到 name 这个字段, 如果有别的用户输入, 就在这里定义

name = serializers.CharField(max_length=10)

```

  

下面是构建 app 下的 url, 然后在 project 先的 url 中引入

```python

# url file under profiles_api

from django.urls import path

from profiles_api import views

  

# 使用 as_view() 方法来调用 APIView

urlpatterns = [

path("hello-view/", views.HelloApiView.as_view()),

]

  

```

注意: 如果是 put, patch, delete 这些方法, 其实他们 PK 是要从用户这里获取的, 所以需要多定义一种 url 类型.

这种定义方式就不适合对有model 访问数据库的方法来做了

```python

urlpatterns = [

path("books/", BookAPIView.as_view()), # list all

path("books/<int:pk>/", BookAPIView.as_view()) # retrieve one

]

```

在项目下的 url 文件中引入 app 下的 url

```python

from django.urls import path, include

# 注意到这里的 urlpatterns 的写法是 django 自带的. 但是后面讲到的 viewset 中的 router 就是 rest_framework的特殊对象

urlpatterns = [

# 新增这一行, api/下面

# 这里用 include 引入 app 下 url 的路径

# 如果访问 api/ 可以看到 app 下所有的 endpoints

path("api/", include("profiles_api.urls")),

]

```

url 定义好之后, 就可以启动服务器, 测试了

  

# ViewSet

ViewSet (High-Level, Resource-Oriented View)

- Encapsulates a set of related views (e.g., list, create, retrieve, update, destroy) into one class.

- You don’t write get/post methods; instead, you implement list(), retrieve(), create(), update(), partial_update(), destroy().

- Usually used with Routers, which automatically generate the URL patterns.

- 多用于 crud 的操作, 和数据库有关

- 不需要想 APIView 那样定义多个不同的 URL pattern, 通过 router 可以自动定义

- HTTP 方法, 对应 URL template, 对应 ViewSet 方法

• GET /books/ → list

• GET /books/{id}/ → retrieve

• POST /books/ → create

• PUT/PATCH /books/{id}/ → update

• DELETE /books/{id}/ → delete

  
  

```python

from rest_framework.routers import DefaultRouter

  

router = DefaultRouter()

# 只要定义这个就可以同时用于 books/<pk> 了

# 注意 r'books' 是 url 的 prefix, basename 是一个 url 的称呼. 两者可以不同.

# basename 可以在代码中被引用, 并通过 reverse('book') 返回 url的前缀 books, 这样, 当 url 变动时, 我们只需要在一处修改. 其他代码都不需要修改

router.register(r'books', BookViewSet, basename='book')

urlpatterns = [

path('', include(router.urls)),

]

```

  

下面我要对之前自定义的用户模型 UserProfile 做一个ViewSet, 使我们可以通过 API 调用来修改他

因为允许用户自己提交和修改数据, 所以就需要为用户输入定义个 Serializer, 在 Rest_frame 中有一个 ModelSeriliazer 专门用于给模型做 Serializer

在 viewset 中的 create, update 等方法是面向 http 请求的, 而在 ModelSerializer 中的 create, update方法是面向对数据模型操作的. 也可以认为是数据库操作. 按照流程来理解就是用户发出 http post 请求, django 收到后, 先调用 viewset 中的 create 方法, viewset中的create 方法调用 Serializer 中的 create 方法, 把数据写入数据库中, 然后 viewset 中的 create 方法, 返回 Response 对象给到用户

```python
from rest_framework import serializers
from profiles_api import models

class UserProfileSerializer(serializers.ModelSerializer):
	"""Serializer a user profile object"""
	class Meta:
		model = models.UserProfile # 这里指定了使用 ModelSerializer 的模型
		# fields 定义了哪些model 中的字段是需要我们做 serializer 的
		fields = ('id', 'email', 'name', 'password')

extra_kwargs = {

'password': {

'write_only': True, # 只能是create 和 update 的时候使用, 不能在 retrive 时使用

'style': {'input_type':'password'} # 指定了HTML 页面上的显示方式, input_type: password 说明不要明文显示. 用****显示

}

}

# 必须要实现 create 和 update 两个方法, 对应 ViewSet 中 Create 和 Update方法

# override这个函数, 就可以用模型中自己定义的create_user方法来构造实例

# 这里参数是 Validated_data

def create(self, validated_data):

"""Create and return a new user"""

# 这个是数据库操作, 在数据库中新建一个对象

user = models.UserProfile.objects.create_user(

email = validated_data['email'],

name = validated_data['name'],

password = validated_data['password']

)

return user

# 这里有两个参数, 一个 instance, 因为要制定修改某一个对象, 一个 validated_data

def update(self, instance, validated_data):

"""Handle updating user account"""

if 'password' in validated_data:

password = validated_data.pop('password')

# 如果要修改密码, 就用模型中的 set_password方法

# 查看模型的定义, AbstractBaseUser类中自带了这个方法

instance.set_password(password)

# 直接调用父类ModelSerializer 中的 update 方法

return super().update(instance, validated_data)

```
这里介绍一下 ModelSerializer 有哪些 validate data 的方法
```
1. Built-in field validation (required, max_length, etc.) # 这是 django 内置的方法
2. Field-level custom methods (validate_<fieldname>) # 定义一个单独的函数, 都某个 field 做验证
3. Object-level custom method (validate) # 可以对多个 field 做交缠验证
4. Model’s own full_clean() (if saving an instance)
既然使用了 ModelSerializer 就也要用到 ModelViewSet
ModelViewSet 会自动帮你写好下面这些函数
```

That’s it — DRF automatically wires up:

• .list() → GET /users/
• .create() → POST /users/
• .retrieve() → GET /users/{pk}/
• .update() → PUT /users/{pk}/
• .partial_update() → PATCH /users/{pk}/
• .destroy() → DELETE /users/{pk}/

```python

from rest_framework import viewsets

  

class UserProfileViewSet(viewsets.ModelViewSet):

# 最少要写两个属性

serializer_class = serializers.UserProfileSerializer # 指定了 serializer

queryset = models.UserProfile.objects.all() # 指定了模型

  

```

定义好 ModelSerializer 和 ModelViewSet 之后, 就写 url 了

```python

# 注意, 只要 ViewSet 中定义了 queryset, 就可以不必加 basename, django 可以根据 queryset 中关联的 model 找到模型

router.register('profile', views.UserProfileViewSet)

  

```

测试 profile, 注意使用过的 id 不能再使用, 即使用户已经被删除了

创建用户

  



  

此时用户可以通过 api 来创建了. 但是出现了几个问题

1. 没有给用户登录的 login 页面

2. 没有控制用户的权限, 谁都能看到所有用户的信息, 即 list 方法, 谁都能修改甚至删除其他用户的信息

  

在课程中, 首先处理的是权限问题

新建一个 permissions.py 文件

```python

from rest_framework import permissions

  
  

class UpdateOwnProfile(permissions.BasePermission):

"""Allow user to edit their own profile"""

# 注意这里的参数, 用户请求 request, view, obj 是当前请求要处理的对象,比如 profile/1

def has_object_permission(self, request, view, obj):

"""Check user is trying to edit their own profile"""

# 这里说明 request 中是有 method 字段的.

if request.method in permissions.SAFE_METHODS: # SAFE_METHODS = ('GET', 'HEAD', 'OPTIONS')

return True # 这里允许用户产看所有的数据

# 下面定义了, 修改数据只能是本人

# 注意这里的返回值, 只有对象的 id 和发起请求的用户的 id 一直, 才有权限

# 这里 django 自己的 authentication中间件决定了在用户发送请求的里使用 request.user.id 这些字段的

# 可以通过 hasattr(request, 'user') 来打印验证

return obj.id == request.user.id

  

```

  

定义某个 viewset 独特的 authentication 方法

```python

  

class UserProfileViewSet(viewsets.ModelViewSet):

# 注意这里定理 authentication 方法是一个 tuple, 且只应用于这个 viewset

# authentication 和 permission 并不相同. ToenAuthentication说的是当用户登录后, django 会生成一个 token 给这个用户

# 然后用户不用再次登录, django 只要验证用户有这个 token, 就完成了使用这个 Endpoint 的身份验证

authentication_classes = (TokenAuthentication,)

# 这里定义了这个 viewset 的权限.也是一个 tuple

permission_classes = (permissions.UpdateOwnProfile,)

  

```

增加 filter 功能

  

```python

class UserProfileViewSet(viewsets.ModelViewSet):



filter_backends = (filters.SearchFilter,) # 可选, 增加 filter 功能

search_fields = ( # 对两个字段进行 filter

"name",

"email",

)

  

```

接下来我们建立一个 login 的 viewset, 这个 viewset 的作用是给一个页面, 让用户登录

用户在登录后, 会返回一个 token, 默认情况下这个 token 会存在数据库里, 不会改变. 所以和密码是一样的

  

```python

from rest_framework.setting import api_setting

  

class UserLoginApiView(ObtainAuthToken):

"""Handle creating user authentication tokens"""

# 默认情况下, 这个 ObtainAuthToken 是返回 json, 定义了下面的 renderer_classes 之后, 就会在 html 页面上呈现

renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES

  

```

用

tigerfanxiao@gmail.com/Fx926926 登录后, 返回如下 token

token 6bb8647d98ba68d36a224f00b58a19493a11a9dd

  

```python

class UserProfileViewSet(viewsets.ModelViewSet):

# 增加这条命令后, 就可以用 token 验证登录了, 可以修改对象

# 没有这条命令的话, 就需要用户用账户密码登录, 再跳转

authentication_classes = (TokenAuthentication,)

```

使用 chrome 浏览器的 ModHeader 插件可以模拟 token, 注意这个插件的使用, 会让正在登录的其他网站登出

输入key/value 是

Authorization: token 6bb8647d98ba68d36a224f00b58a19493a11a9dd

  

# 创建一个 feed 的 model 和 viewset

  

```python

# profiles_api/models.py

from django.conf import settings # 引入 settings 文件

  

class ProfileFeedItem(models.Model):

"""Profile status update"""

# 这里定义了一个外键, 关联默认的

user_profile = models.ForeignKey(

settings.AUTH_USER_MODEL, # 这里是最佳实践, 使用 setting 中的变量. 一般情况下是用 model 的名字, 文本形式

on_delete=models.CASCADE,

)

status_text = models.CharField(max_length=255)

created_on = models.DateTimeField(auto_now_add=True)

  

def __str__(self):

"""Return the model as a string"""

return self.status_text

  

```

因为这里创建了 model, 所以需要用 migration

  

```python

python manage.py makemigrations

python manage.py migrate

```

然后在 admin 页面注册这个 model, 就可以在 ui 中管理了

```python

from django.contrib import admin

from profiles_api import models

  

admin.site.register(models.ProfileFeedItem)

  

```

如果要构建API endpoint 就需要创建 serializer 和 Viewset

  

```python

  

class ProfileFeedItemSerializer(serializers.ModelSerializer):

"""Serializes profile feed item"""

class Meta:

model = models.ProfileFeedItem

fields = ('id', 'user_profile', 'status_text', 'created_on')

extra_kwargs = {'user_profile': {'read_only': True}} # 不希望用户自己修改, 而是从认证信息中获得

```

  

这里有个有趣的东西 perform_create

In Django REST Framework (DRF), perform_create is a hook method that gets called after your serializer is validated, but before the HTTP response is returned — specifically during the create() flow of a ViewSet or GenericAPIView.

  

It’s basically a place for you to add extra logic right after saving, without having to fully rewrite the create() method.

  

Where it fits in the flow

  

When a POST request hits a view that inherits from DRF’s mixins:

1. Serializer is created with incoming request.data.

2. .is_valid() is called — validation happens.

3. If valid, DRF calls perform_create(serializer).

4. By default, perform_create() just calls serializer.save().

5. DRF builds and returns the HTTP 201 Created response.

  

```python

class UserProfileFeedViewSet(viewsets.ModelViewSet):

"""Handle creating, reading and updating profile feed items"""

authentication_classes = (TokenAuthentication,)

serializer_class = serializers.ProfileFeedItemSerializer

queryset = models.ProfileFeedItem.objects.all()

permission_classes = (

permissions.UpdateOwnStatus,

IsAuthenticated, # 如果没有认证身份则无法查看, IsAuthenticatedOrReadOnly 如果没有认证则可以看, 不能修改

)

  

def perform_create(self, serializer):

"""Sets the user profile to the logged in user"""

# 这里把用户的 request.user 传进去, 而不是把 request.data["user_profile"]传进去

# 因为我们在上面定义了 authentication_classes = (TokenAuthentication,), 所以用户必然是受过验证的, 所以一定有request.user 存在

serializer.save(user_profile=self.request.user)

  

```

注意, 这里还有 permission 的要求, 怎么要求 UpdateOwnStatus, 和之前的 UpdateOwnProfile 一样

```python

  

class UpdateOwnStatus(permissions.BasePermission):

"""Allow user to update their own status"""

def has_object_permission(self, request, view, obj):

"""Check the user is trying to update their own status"""

if request.method in permissions.SAFE_METHODS:

return True

return obj.user_profile.id == request.user.id

```

  
  

既然 viewset 都定义好了, 最后就是定义 URL

  

```python

  
  

```

  
  

# 感想

- 在使用 chatgpt 的情况下, 是加快了我们学习的速度. 但是也会让人失去思考能力, 让人产生一种我已经会了的错觉.

就好像看着数学习题集的答案来做题. 当人们遇到真的问题时, 可能还是毫无办法.

chatgpt 是一种即使反馈的机制, 帮助很大, 但是也要用好这种机制, 迅速发现自己不足的地方, 然后还是不要放弃用刻意记忆去弥补自己的不足, 达到能力增长的目的

- api 的搭建除了本身功能性的部分, 主要指业务逻辑方面. 另外一个大部分就是权限管理, 需要确定哪些人能用 API, 哪些人能看到哪些内容.

- 这部分权限管理应该在一个权限管理的中台统一管理, 即 SSO, 类似 AWS IAM, 也称为 Authenticate 的中台. 后面的项目应该考虑这部分的时间 jwt, auth2.0 之类的东西

-

  

## 这个项目没有讲的地方

  

### 其他数据库引擎.

在 settings.py 在数据库的部分, 其实是给了一个网址, 提供了连接其他数据库的模板

https://docs.djangoproject.com/en/2.2/ref/settings/#databases

  

### 日志

这个项目没有定义日志的打印方式
