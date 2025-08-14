# Thoughts
技术本质上是为了商业服务的, 至少目前我的这个情况下. 我们这是用代码去实现商业的价值. 从这个意义上看, 我们是来解决问题的. 要解决问题就要看需要实现的目标和成本. 在目标和成本有一个比较清洗的认识之后, 我们才能选定技术方案

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
- 能用Firebase和DRF合作吗. 本质是Firebase是一个基本能取代django rest framework的东西, 只要前段就好了
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
		
# Django
Django和Django Restful 的关系是什么. 其实Django和Django Restful是不同的包. 因为在开发的过程中, 我们可以看到有些包使用Django中直接import的, 有些包是从Rest Framework中import的, 注意区分
todo: 哪些包是Rest Framework中的能
### Framework
```shell
# requirements.txt
django==2.2 # django framework 2019
djangorestframework=3.9.2 # drf framework 2019

```
### Develop environment

1. 使用venv模块创建python虚拟环境
2. 通过requirements.txt 安装
```shell
# 制定虚拟环境的路径, 非本地路径
python -m venv ~/env

# 在非本地路径激活虚拟环境
source ~/env/bin/activate
# 退出虚拟环境
deactivate
```

通过`requirements.txt` 安装虚拟环境的包
```shell
# 在虚拟环境被激活的情况下
pip install -r requirements.txt 
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
## Servers
区分几个概念
- 服务器分为Application Server和Web-server两种
- 在python中, Application Server 有wsgi和asgi 两种, 即asgi可以处理异步请求, wsgi不行
- wsgi和asgi都是Application Server的接口规范, 不是产品本身
- Application Server 往往部署在 Web-server的后面
- 市场上成熟的wsgi服务器有 uWSGI, Gunicorn ASGI 服务器有 Uvicorn
- wsgi和asgi都是都提供python的运行环境, 就是说, python通过接受到达这些服务器的request, 来处理和响应
- 无论是wsgi还是asgi 其实都是开发中使用的服务器. 真正在生产环境中, 其实还有一种服务器类型叫 web-server, 这种服务器本身用来承接海量的需求. 将承接到的需求是转交给wsgi/asgi服务器来处理. 如果需求只是获取静态文件, 那么这种需求就可以直接到static文件的存储中提取文件反馈给用户, 不需要进过python wsgi和asgi 服务器来处理. 这也就解释了 `python manage.py collectstatic --noinput` 的作用. 在实际生产环境部署中, ngnix需要知道静态内容的存储路径的. 这些内容应该在nginx的配置文件中写明

### Nginx
学习方法: 在学习完django开发后, 学习部署时才专门学习
web-components:
- Forward Proxy 站在用户的前面, 替用户发出请求Internet. 
	- Access Control: Block access to certain website, or restrict internet usage within a company
	- Security: Scan for any viruses and block
	- Monitoring: In theory, it could log employee's web activity
	- Caching Response
- Reverse Proxy, 站在服务器的前面, 替服务器去接受来自Internet的用户请求
	- Load Balancer 站在服务器的前面, 将流量分给不同的服务器
	- Protect Servers: Expose proxy server as entry point. Configure security measures on the proxies
	- Ensure SSL encryption is enabled 
	- Caching: 加速用户访问
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
6. 把model注册到 Django admin
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

# Operation
### Vagrant
```shell
config.vm.box = "ubuntu/bionic64" 
# 切换到
config.vm.box = "ubuntu/jammy64"

```

```shell

# 关闭当前的虚拟机
vagrant hault

# 删除现在的虚拟机
vagrant destroy 

# 修改vagrantfile之后, 重新创建虚拟机
vagrant up
```

### Git
1. 首先在github上创建项目
2. 从github上把项目拉倒本地 ??
```shell
# git 项目初始化
git clone <link> <target_path>

# 每次更新代码
git add .
git commit -am "Added user profile API"
git push origin main

# 修改刚提交的commit的说明
git commit --amend -m "changed description"
# 查看之前提交的说明
git log --oneline
```

在github上的查看提交的commit 次数和历史
![[Pasted image 20250802231153.png]]
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
