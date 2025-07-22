Docker Compose 是 docker 之外的服务

```shell
docker compose -v 
```
### `Dockerfile`  VS. `docker-compose`

- `Dockerfile` 是构建镜像用的. Docker Compose 将镜像构建容器或者服务
- `Dockerfile` 和 `docker-compose` 不一定要放在同一个文件目录下
-  ports 一定要加引号, 否则会造成服务器正常被访问


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

### 容器化开发
- 怎么在开发阶段看到执行错误信息
- 构造 `Dockerfile`用于开发环境, `Dockerfile.prod`用于生产环境
- 开发环境有一些特殊的包 
```yaml
version: '3.8'

services:
  web: # service name
    build: ./project #  Dockerfile
    command: uvicorn app.main:app --reload --workers 1 --host 0.0.0.0 --port 8000
    volumes:
      - ./project:/usr/src/app
    ports:
      - 8004:8000
    environment:
      - ENVIRONMENT=dev
      - TESTING=0

```

### Postgres container
- environment 可以通过在项目目录下创建 `env_vars/postgres.env` 文件存放. 在的 `docker-compose.yaml` 中, 使用 `env_file: - ./env_vars/postgres.env` 指定环境变量文件
- adminer 是一个用于访问postgres 的客户端
- 这里的`/docker-entrypoint-initdb.d`目录是一个特殊的目录, 一般数据库的container都有整个目录用来运行构建数据库的脚本. 注意目录下的脚本需要添加执行权限(mac)
```yaml
version: "3.9"
services: 
	db: #
		image: postgres:16.1-alphine3.19
		restart: always
		environment:
			- POSTGRES_USER=postgres
			- POSTGRES_PASSWORD=postgres
		port: 
			- "5432:5432"
		volumes:
			- ./scripts:/docker-entrypoint-initdb.d
	adminer:
		image: adminer
		restart: always
		port: 
			- "8080:8080"
```
构建 `scripts/db-setup.sh`
```shell
#!/bin/sh
export PGUSER="postgre"
psql -c "CREATE DATABASE <database>" # -c 表示运行后面的命令
psql <database> -c "CREATE EXTENSION IF NOT EXISTS \"uuid-ossp\";" # 为数据库增加uuid插件
```
### Docker compose up
- 创建 `docker-compose.yml` 文件
- docker compose 命令会让container的log日志定在terminal里

```shell

# 构建镜像, 不运行 container
docker compose build 

# 重构镜像, 运行容器
# -d 运行容器 detach 模式, 否则 terminal 会顶在那里
docker compose up -d --build
# 指定 docker-compose 文件来启动进项
docker compose up -d -f docker-compose.yml

# 关闭容器
docker compose down 
```
### docker compose exec
```shell
# 访问容器中 postgres
docker compose exec <container_name> psql -U postgres
```


```yaml
# 这个容器的启动依赖另一个容器吗
depends_on: 
	- "dyt-django"

restart: always # 如果因为依赖的容器没起来, 自己也没起来, 就会自动重启
```