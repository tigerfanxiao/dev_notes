# Windows
安装8.4 版本
下载windows 安装包
默认安装
`C:\Program Files\MySQL\MySQL Server 8.4\bin` 目录下, 需要将这个目录放入环境变量

mysqld 是服务, 通过后台可以查看
mysql 是客户端

```shell
# 使用管理员权限打开terminal
# 初始化 mysql, 创建 data 文件夹
	C:\mysql-8.0.46-winx64\bin>mysqld --console --initialize
2026-06-06T09:01:37.456087Z 0 [System] [MY-013169] [Server] C:\mysql-8.0.46-winx64\bin\mysqld.exe (mysqld 8.0.46) initializing of server in progress as process 32592
2026-06-06T09:01:37.472803Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2026-06-06T09:01:37.623305Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2026-06-06T09:01:38.326179Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: n:jxe/FqM7ty # 这里有root的临时密码
# 查看mysqld 的所有命令
mysqld --help --verbose > help.txt # 这里有信息注册成服务

# 启动mysql
C:\mysql-8.0.46-winx64\bin>mysqld --console
2026-06-06T09:02:52.665115Z 0 [System] [MY-010116] [Server] C:\mysql-8.0.46-winx64\bin\mysqld.exe (mysqld 8.0.46) starting as process 17924
2026-06-06T09:02:52.677820Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2026-06-06T09:02:52.869817Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2026-06-06T09:02:53.108131Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2026-06-06T09:02:53.108359Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2026-06-06T09:02:53.134968Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060
2026-06-06T09:02:53.135058Z 0 [System] [MY-010931] [Server] C:\mysql-8.0.46-winx64\bin\mysqld.exe: ready for connections. Version: '8.0.46'  socket: ''  port: 3306  MySQL Community Server - GPL.


net stop mysql # 手动停止mysql 服务
netstat -an -p tcp # 查看所有tcp 协议的端口, 看3306 是否被占用

mysql_secure_installation.exe
# 使用之前的临时密码登录, 修改当前密码

# 初始化之后是拒绝本地用户访问的
mysql -u root -p # 使用客户端连接mysql

create user 'xiao'@'localhost' identified by 'Fx123456';
grant all privileges on *.* to 'xiao'@'localhost' with grant option; # 前面一个 * 代表数据库, 后面一个 * 代表所有的表
flush privileges;

select user, host from mysql.users;
```


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