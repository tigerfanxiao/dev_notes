
docker compose 文件
```yaml

version: '3.1'
services:
	qytpg:
		image: postgres:13.7
		container_name: qytpg
		ports:
			- "5432:5432"
		environment:
			POSTGRES_USER: gytangdbuser
			POSTGRES_PASSWORD: Cisc0123
			POSTGRES_DB: gytangdb
		volumes:
			- ./pg_hba.conf:/etc/postgresql/pg_hba.conf
			- ./postgresql.conf:/etc/postgresql/postgresql.conf
		command:
			-c config_file=/etc/postgresql/postgresql.conf
			-c hba_file=/etc/postgresql/pg_hba.conf

```


运行docker compose

```shell
docker compose up -d
# 第一次运行需要下载镜像
docker ps

```