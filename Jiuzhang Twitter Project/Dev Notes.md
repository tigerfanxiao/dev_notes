[老师的repo](https://github.com/jiuzhangsuanfa-org/django-twitter)

项目的管理员 admin / fx926926

视频录播 + 材料 37G
- [x] 复制九章twitter项目 

# 安装和初始化MySQL
mysql root账户密码 123456

mysql 在本地安装的， 需要安装插件 mysql_native_passworc 才能用第三方工具比如vscode中的mysql来访问
比如使用MySQL Workbench
```sql
-- 修改root用户
ALTER user 'root'@'localhost' identified with 
mysql_native_password BY '123456'

-- 如果是添加新的用户
CREATE USER 'newuser'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';

```

安装 mysql client
第三方的`mycli`

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
需要在windows上安装less来解决， 或者使用gitbash作为terminal
需要把less的路径`C:\Program Files (x86)\GnuWin32\bin\`添加到环境变量

