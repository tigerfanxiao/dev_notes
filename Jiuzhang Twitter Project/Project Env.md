
python 本地版本 3.10

docker开发策略
在本地做一个python版本，然后用docker再维护一个相同的docker版本
每次在本地编译，然后同步到docker中去

# 步骤
1. 安装python版本
2. 安装requirements
3. 创建django项目



# requirements
一般做一个生产环境和开发环境的requirements.txt
问题： 有些包不知道具体的作用

```shell
# requirements-prd.txt
Django==3.1.3  
pymysql   # 用于替代django中默认使用的mysqlclient包，这个包更好用，但需要配置__init__.py
```
只在开发环境才装
```shell
# requirements-dev.txt
mycli  # 在python的虚拟环境下，用于连接mysql的客户端工具，有自动补全和语法高亮
ipython  #?
autopep8  # python的语法规则
psutil #?
```

安装包
```shell
python -m pip install -r requirements-prd.txt
python -m pip install -r requirements-dev.txt
python -m pip install --upgrade pip
```

# 创建django项目
* 项目名为twitter

```shell
django-admin startproject twitter .  # . 表示在本地目录创建，不会多创建一层twitter/twitter目录
```

在本地启动项目
```shell
python manage.py runserver 0.0.0.0:8000
# 此时数据库使用的是sqlite
```

到此完成在本地创建项目

# 安装和初始化MySQL
## 本地安装MySQL
*  下载[MySQL 8.0 安装包](https://dev.mysql.com/downloads/installer/)
* 下载并安装 `visual code .net`工具包

![[Pasted image 20230321060422.png]]

因为公司电脑不允许使用docker， 故在本地安装MySQL

mysql root账户密码 123456

## 通过vscode也访问MySQL
mysql 在本地安装的， 需要安装插件 mysql_native_passworc 才能用第三方工具比如vscode中的mysql来访问
比如使用MySQL Workbench
```sql
-- 修改root用户
ALTER user 'root'@'localhost' identified with 
mysql_native_password BY '123456'

-- 如果是添加新的用户
CREATE USER 'newuser'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';

```

## 安装 mysql client
第三方的`mycli`允许在terminal访问呢mysql

> 为什么用 mycli，而不是 mysql 官方的 client 呢？因为单独安装官方的 mysql client 麻烦，而且也不好用。mycli 是 python 写的第三方客户端，带有：
> 
> -   命令补全
> -   智能提示
> -   色彩高亮
> -   高危风险提示 等等功能, 是非常优秀的替代品

pip 安装
```shell
pip install -U mycli
```

配置django
```python
DATABASES = {  
	'default': {  
	'ENGINE': 'django.db.backends.mysql',  
	'NAME': 'twitter',  
	'USER': 'root',  
	'PASSWORD': '12345',  
	'HOST': 'localhost',  
	'PORT': '3306',  
	}  
}
```

在terminal使用mycli连接数据库 
```shell
# win10
# 连接到mysql
mycli -u root -h localhost -p 123456
# 连接指定数据库
mycli -u root -h localhost -p 123456 --database twitter

```

登录后， 在window上会遇到问题
```shell
MySQL root@localhost:(none)> show databases;
'less' is not recognized as an internal or external command,
operable program or batch file.                             
7 rows in set                                               
Time: 0.011s                                                
MySQL root@localhost:(none)>

```
需要在windows上安装[less](https://gnuwin32.sourceforge.net/packages/less.htm)来解决， 或者使用gitbash作为terminal
需要把less的路径`C:\Program Files (x86)\GnuWin32\bin\`添加到环境变量

此时可以联通数据库
```shell
(venv) PS D:\XIAO\Learning\Jiuzhang_Twitter\my_django-twitter> mycli -u root -h localhost -p 123456
MySQL 
mycli 1.26.1
Home: http://mycli.net
Bug tracker: https://github.com/dbcli/mycli/issues
Thanks to the contributor - Heath Naylor
MySQL root@localhost:(none)>

```

# django配置MySQL

首先配置`settings.py`
```python
DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.mysql',  
        'NAME': 'twitter',  
        'USER': 'root',  
        'PASSWORD': '123456',  
        'HOST': 'localhost',  
        'PORT': '3306',  
    }  
}
```
但是django默认会指定`mysqlclient`作为数据库driver。 如果直接启动server会报错

启动django还是会遇到问题

```shell
  File "D:\XIAO\Learning\Jiuzhang_Twitter\my_django-twitter\venv\lib\site-packages\django\db\backends\mysql\base.py", line 17, in <module>
    raise ImproperlyConfigured(
django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module.
Did you install mysqlclient?

```
因为mysqlclient不好用，我们这里用`pymysql`, 但是django默认是使用mysqlclient的， 所以在加载程序的时候， 修改

```python
# 在setting.py同路径下的 __init__.py 中添加
import pymysql  
pymysql.install_as_MySQLdb()
```
启动django
```shell
python manage.py runserver 0.0.0.0:8000
```

初始化数据库
```shell
python manage.py migrate
python manage.py makemigrations
```

此时通过mycli查看`twitter`数据库
```shell
+----------------------------+
| Tables_in_twitter          |
+----------------------------+
| auth_group                 |
| auth_group_permissions     |
| auth_permission            |
| auth_user                  |
| auth_user_groups           |
| auth_user_user_permissions |
| django_admin_log           |
| django_content_type        |
| django_migrations          |
| django_session             |
+----------------------------+

10 rows in set

```

