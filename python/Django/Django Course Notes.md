
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
django test suite 是 django 自带的 unit test 模块
使用 docker compose 命令来执行 unit test
```shell
docker compose run --rm app sh -c "python manage.py test"
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

github action
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


# django

创建项目 app, 虽然这里是 --rm 会删除 container, 但是因为 volume 打通了, 创建的文件会留下来
```shell
docker compose run --rm app sh -c "django-admin startproject app ."

# 如果要运行容器不关闭
docker compose up
```
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

