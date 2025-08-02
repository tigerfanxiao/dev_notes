
# Thoughts
技术本质上是为了商业服务的, 至少目前我的这个情况下. 我们这是用代码去实现商业的价值. 从这个意义上看, 我们是来解决问题的. 要解决问题就要看需要实现的目标和成本. 在目标和成本有一个比较清洗的认识之后, 我们才能选定技术方案

如果要做CIO, 需要会做预算, 每年在IT成本上的投入和整体的企业营收之间的关系. 如果向CTO去解释, 我们需要多少经费来实现怎样的目的

从跟目前这个项目看开发和部署确实是两种完全不同的活, 都要花非常多的时间. 所以需要用CICD, 把部署自动化, 这样就可以把时间专注在开发上了

### Questions
- supervisorctl 在linux 中是什么工具?
- `python manage.py collectstatic --noinput` 的效果
### Move on
- Development
	- 这个项目没有写怎么开发Log, 记录程序的一些动作, 比如用户登录了, 用户的行为, 用于排错
	- 这个项目没有Exception
	- 这个项目没有Unit Test
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
	- Python 语法, Comprehension list, 正则
	- Python Package
	- Python API 开发
		- Django Rest Framework 基本框架
		- Django 框架
		- UnitTest 框架
- Database
	- Sqlite
	- Postgres
- Devops
	- Basic Linux
	- Git/Github 
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
	- 云计算
		- AWS 
			- Coursera上的 Data Engineer 课程
			- AWS Data Engineer Associate Certificate 获得
		- Azure
		- GCG
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


运行开发环境的服务器
```shell
python manage.py runserver 0.0.0.0:8000
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
把模型注册到Django admin中
```python
from django.contrib import admin
from profile_api import models # 从自定义的app中引入models.py

# 注册模型
admin.site.register(models.ProfileFeedItem) # 自定义的模型
```
在注册完成后, 可以在admin中对应的app下, 看到model的名字
![[Pasted image 20250802203527.png]]
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
		fieldS = ('id', 'user_profile', 'status_text', 'created_on')
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

### Git
```shell
# git 项目初始化
git clone <link> <target_path>

# 每次更新代码
git add .
git commit -am "Added user profile API"
git push origin main
```

在github上的查看提交的commit 次数和历史
![[Pasted image 20250802231153.png]]
### AWS
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
### supervisorctl
用于控制Linux上的进程
```shell
environment = 
	DEBUG=0 # 给系统这是环境变量
```


# Others
HTTP Status
400 页面不催在
401 Authentication error

302 页面跳转
