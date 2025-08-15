

# Aurora

Aurora 可以 Autoscaling
Aurora Replica 是备份在另一个 region 的

如果是跨区域的灾备, 使用 Aurora Global Database

可以通过 Activity Stream来监控数据库状态

Amazon Aurora is belong to RDS
Amzon Aurora is 5 times faster than MySQL
By default, RDS has 16TB volume
Two key features
-   Multi-AZ-For Disaster Recovery
-   Read Replicas-For Performance
-   6 copies by default

最多可以有 15 个 read replica

## RDS

- 默认情况下, 每天都会有 30 分钟备份快照, 你可以指定时间, 选择读写压力小的时候. 不会断网, 但是会影响性能
- 如果需要创建多AZ 的 RDS 实例, 需要创建 Subnet Group

### Connect RDS

使用 EC2 直接连接 RDS postgre
```shell
yum install -y postgresql
# 默认用户名是 postgres
psql -U postgres -H <rds_host_for_writing>
# input password
# 进入 rds
create database <database_name>
create user <user_name> # 创建一个新用户
# 输入密码
# 给新用户增加指定数据的权限
grant all on database <database_name> to <user_name> 
# 退出连接
\q
```



使用 python 连接 RDS postgre
```shell
# 登录 EC 2
yum install python3
# 安装 python 使用的包
pip3 install psycopg2-binary 

```


```python
import psycopg2 as pg2
rds = <rds_hostname_for_writing>
conn = pg2.connect(host=rds, user=<user_name>, password=<password>, database=<database_name>)
cursor = conn.cursor()
# 创建表格, 插入数据
cursor.execute("Create Table test(t1 int, t2 vachar(40))")
cursor.execute("Insert Into test (t1, t2) values (11, 'welcome xiao')")
cursor.execute("Insert Into test (t1, t2) values (12, 'welcome Fan')")

# 查询结果
cursor.execute("select * from test")
result = cursor.fetchall()

for i in result:
	print(i)
# 提交
conn.commit()
conn.close()
```


# DynamoDB
自带auto-scaling
如果请求量徒增, 可以使用 DynamoDB Accelerator DAX来加速