
python 本地版本 3.10

docker开发策略
在本地做一个python版本，然后用docker再维护一个相同的docker版本
每次在本地编译，然后同步到docker中去

# 步骤
1. 安装python版本
2. 安装requirements
3. 创建django项目
# Python 版本

问题： 使用pipfile.lock开控制版本？ 这是pyenv中的东西吧。
在win10上， python可以允许安装多个版本的。 所以在pycharm中可以配置多个版本
在命令行， 可以用特殊的命令来控制执行器的版本？


# requirements
一般做一个生产环境和开发环境的requirements.txt
问题： 有些包不知道具体的作用

```shell
# requirements-prd.txt
Django==3.1.3  
pymysql
```
只在开发环境才装
```shell
# requirements-dev.txt
mycli  
ipython  #?
autopep8  
psutil #?
```

安装包
```shell
python -m pip install -r requirements-prd.txt
python -m pip install -r requirements-dev.txt
python -m pip install --upgrade pip
```

# 创建django项目

```shell
django-admin startproject twitter .
```

在本地启动项目
```shell
python manage.py runserver 0.0.0.0:8000
```

到此完成在本地创建项目

下面做docker部分
安装 mysql、redis，容器
