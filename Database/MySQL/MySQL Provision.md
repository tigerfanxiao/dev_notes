
```shell
sudo apt-get update
sudo apt-get install mysql-server


# 不是必须的安装 mysql utility tool
sudo mysql_secure_installation utility


# 允许远程访问
sudo ufw enable 
sudo ufw allow mysql

# 重启启动 mysql
sudo systemctl start mysql
```


```shell

mysql -uroot
# 输入密码后登录

exit # 退出mysql

```


# MySql 回复Root密码

```shell

mysql -V

sudo /etc/init.d/mysql stop
sudo mkdir /var/run/mysqld
sudo chown mysql /var/run/mysqld
sudo mysqld_safe --skip-grant-table&
sudo service mysql start
sudo /usr/bin/mysql --user=root mysql

mysql> UPDATE mysql.user SET authentication_string=null WHERE User='root';
mysql> flush privileges;
mysql> exit

```