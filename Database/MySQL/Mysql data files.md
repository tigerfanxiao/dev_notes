Mysql 的文件是存在 `/var/lib/mysql` 下的. 当磁盘写满是, mysql 的服务就挂了

```shell
sudo systemctl stop mysql
sudo rsync -av /var/lib/mysql /mnt/new_mysql_data

sudo rsync -av --dry-run /var/lib/mysql /mnt/new_mysql_data


```

修改mysql 配置文件 `/etc/mysql/mysql.conf.d/mysqld.cnf`

```shell
[mysqld]
datadir = /mnt/new_mysql_data/mysql
```


```shell
sudo chown -R mysql:mysql /mnt/new_mysql_data/mysql
sudo systemctl start mysql

# 删除老的 mysql 数据
sudo rm -rf /var/lib/mysql.old 

```