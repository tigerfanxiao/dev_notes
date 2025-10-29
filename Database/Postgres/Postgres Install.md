- 默认的端口上 5432
安装服务端
```shell
yum install posgresql-devel
```
安装客户端
```shell

yum install posgre
# 连接客户端, 默认的管理员 postgres, 默认的数据库 postgres
psql -U postgres -d <database_name> 172.17.0.2
# 键入密码
```

使用python 访问postgre
```shell
pip3 install psycopg2-binary
```
# 数据库查询命令
```shell
\l # 查看所有的数据库
\c webdeb # 连接 webdeb 数据库
\dt # 查看 webdeb中的所有表格
\q # 退出postgre
```

