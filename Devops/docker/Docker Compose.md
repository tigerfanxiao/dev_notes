Docker Compose 是 docker 之外的服务

```shell
docker compose -v 
```
### `Dockerfile`  VS. `docker-compose`
如果我们只是构建一个镜像, 那用 `Dockerfile`就足够了. 如果是构建一组容器, 那就用docker-compose
- `Dockerfile` 是构建镜像用的. Docker Compose 将镜像构建容器或者服务
- `Dockerfile` 和 `docker-compose` 不一定要放在同一个文件目录下


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
    build: ./project #  构建进项的 Dockerfile
    command: uvicorn app.main:app --reload --workers 1 --host 0.0.0.0 --port 8000
    volumes:
      - ./project:/usr/src/app
    ports:
      - 8004:8000
    environment:
      - ENVIRONMENT=dev
      - TESTING=0

```

postgres container
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
	adminer:
		image: adminer
		restart: always
		port: 
			- "8080:8080"
```

### Docker compose up
首先要创建 `docker-compose.yml` 文件

```shell
# 当前目录下必须有文件 docker-compose.yml
# 只是 build image, 不运行 container
docker compose build 
# 重构镜像, 启动容器
# -d 运行容器 detach 模式, 否则 terminal 会顶在那里
docker compose up -d --build
# 指定 docker-compose 文件来启动进项
docker compose up -d -f docker-compose.yml
# 关闭 docker compose 想关的容器
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