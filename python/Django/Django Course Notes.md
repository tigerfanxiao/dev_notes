
目标
1. 跟着视频完整做一遍
2. 不看视频, 根据自己的笔记能够完整做一遍
3. 精简文档, 快速开发做一遍. 看看这个项目有没有扩展的地方. 变体或者和其他项目打通

API Feature
- user authentication 
- creating object
- Filtering & sorting objects
- uploading & viewing images

django 3.2 
怎样升级 api



# unit test

> [!key]
使用 docker 做一次性的用例测试 
```shell
docker-compose run app sh -c "python manage.py test"
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

django 项目目录结构
```
app/ # Django project
app/core/ # Code shared between multiple apps
app/user # User related code
app/recipe # Receip related code

```

### TDD
TDD is software development practice. 特点是先写根据 feature 写测试用例, 在写开发代码

### docker
从 docker 20.10 开始 docker-compose 命令集成到了 docker 中, 所以所有的 docker-compose 命令可以用 docker compose 命令来替代

dockerfile 和 docker-compose 的区别
- dockerfile 在操作系统层面定义的 image 的制作
- docker-compose则是定了如何运行这些由 dockerfile 定义的镜像 image
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

### git
构建 github repo recipe-app-api
 一般先在 github 上创建 repo, github 上提供了, 基于编程语言个 gitignore 文件和不同的 License 选择

# 其他想法
### Pandas
复习之前在 kaggle 上考的 pandas 证书
https://www.kaggle.com/learn/pandas
[[Python Pandas]]
### SQL 即其他免费的学习资源
学习在 kaggle 上的 sql 课程并获得证书
https://www.kaggle.com/learn

