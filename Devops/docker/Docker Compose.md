
Dockerfile 和 Docker Compose 的区别
- Dockerfile 是构建镜像用的. Docker Compose 是构建容器的, 里面有环境变量, 网络
- Dockerfile是 json 文件. Docker compose 是 yaml 文件


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


首先要创建 `docker-compose.yml` 文件

```shell
# 文件名必须是 docker-compose.yml
# build image
docker-compose build 

# 运行容器 detach 模式
docker compose up -d
```
运行docker compose

```shell

# 指定文件名
docker compose up -d -f docker-compose.yml

# 关闭 docker compose 想关的容器
docker compose down 

docker ps



```



```yml

depends_on: 
	- "dyt-django"

restart: always # 如果因为依赖的容器没起来, 自己也没起来, 就会自动重启
```