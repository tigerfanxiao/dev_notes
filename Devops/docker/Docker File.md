
## 镜像的分层
一个镜像是又很多层构成, 每一层称为一个 Layer. 在构建镜像的时候, 每个 `RUN`命令会创建一个 layer, 而每一个 layer 都会扩大镜像的体积, 所以一般在 `RUN` 命中放了很多用`&&`连接的命令
如果要构建一个镜像. 创建`Dockerfile` 文件
`FROM` 命令这里用的是基础镜像. 基础镜像一般也有很多层了. 新的镜像会讲 Layer 放在后面. 

* 删除镜像中的内容, 并不是真的删除. 而是叠加新的一层, 伪装成删除的样子. 这是 AUFS 的特性. 一种类似 git 所使用的文件系统. 

注意: Image 文件的每一行都等于一层
# 步骤
1. 创建`Dockerfile` 文件
2. 执行 `docker build`命令来构建容器
3. 通过镜像来创建容器

 Case: Make Python FastAPI docker development environment
 1. Create a `Dockerfile` 
``` Dockerfile
# Base image to specify the python version
FROM python:3.14.0b1-slim-bullseye
# Make a working directory
WORKDIR /app
# Copy and install requirements.txt to the container 
# The installation will be cached by docker itself. Suppose we don't change the requirement as frequently as code, each time we change the docker file it, this package installation would be re-run
# --no-cache-dir means not save the python package inside of container
COPY ./requirements.txt /app
RUN pip3 install --no-cache-dir -r requirements.txt
# copy the local source code to container
COPY . /app
# run flask server
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "-port", "8000", "--reload"]
```
2. Build image from the `Dockerfile`
```shell
# . means find Dcokerfile in the current directory
# -t means tag. If you didn't assign a tag, docker daemon will randomly assign it, which helps you later on easily find this image
docker build -t xiao/flask_api:0.1 .
```
3. run docker container 
```shell
# channel-api is image name
# -d means detached 
docker run -d -p 8080:80 channel-api 

# kill container
docker kill <container_id>
```
4. Test on local browser `http://localhost:8080/`
5. Use docker compose to automate the process
	1. Create a `docker-compose.yaml` file 
```YAML
services:
  app:
    build: . # Directory of Dockerfile
    container_name: simple-python-server
    # the command will replace the CMD in Dockerfile restart the server 
    # uvicorn only detect the change on .py file
    command: uvicorn main:app --host 0.0.0.0 --port 80 --reload
    ports:
      - 8080:80 # Local on windows port 8080 mapping to docker container port 80
    # Any change in volumes will trigger reload uvicorn server, overwrite the copy command in Dockerfile
    volumes:
      - .:/app
```



```shell
docker compose up --build # first time you need to use --build
```

```shell

#  -rm means delete the container after it run
# 传入环境变量 IF_NAME=lo0
docker run --rm --name qyt-dockerfile -e IF_NAME=lo0 xiao/centos_ifconfig
# 最后传入的命令, 可以把定义在 CMD 中的命令覆盖掉
docker run --rm --name qyt-dockerfilexiao/centos_ifconfig ls 

```
如果是自己命名的dockerfile文件
```shell
# 在当前目录中找指定文件名的 Dockerfile 文件
docker build -t xiao/image_name:0.1 -f my_docker_file_name.Dockerfile .
```
### `Dockerfile` 的命令

```dockerfile
FROM # 这个镜像的妈妈是谁? 指定基础镜像
MAINTAINER # 告诉别人, 谁负责养它? 指定维护者信息
RUN # 想让它干啥 在命令面前加上 RUN 即可
ADD # 给他点创业资金 COPY 文件, 会自动解压
COPY # 复制文件, 用的比 ADD 多
WORKDIR # cd 到工作目录
VOLUME # 设置卷, 挂载主机目录
ENV # 设置环境变量
EXPOSE # 声明提示要暴露的端口号, 没什么用. 更多的是给别人看文档用的. 真正起作用的是 docker run 设定
CMD # 指定容器启动后要做的事情
ENTRYPOINT # 类似于 CMD, 但是不会被 docker run 最后的命令覆盖, 且最后输入的内容会作为 Entrypoint 中命令的参数传递进去
```


常见的 RUN 命令
``` dockerfile
FROM centos:8
# 安装python的依赖包
RUN pip3 install -r requirements.txt 
# 安装 python运行环境
RUN yum -y update && \
	yum -y install gcc python3-devel && \
	yum -y install python3 && \
	yum -y install net-tools && \
	yum -y install dos2unix
# 安装 nginx
RUN yum -y update && \
	yum -y install vim net-tools && \
	yum install -y epel-release && \
	yum -y install nginx

```

本地复制文件

```dockerfile
# 会自动解压并复制
ADD test.tar.gz /qytang
# 单纯的复制文件, 用的比 ADD 多
COPY file /qytang 
```

环境变量
```dockerfile
# 等号表示默认值
ENV IF_NAME=eth0 SSH_PORT=22 
# 必须强制要传值
ENV <key> <value> 
```

### Volume
```dockerfile
# 把/qytang 的中文件挂载到镜像上
VOLUME /qytang 
```

### `Entrypoint`
且最后输入的内容会作为 Entrypoint 中命令的参数传递进去
```Dockerfile
ENTRYPOINT ["ifconfig"]
```

构建容器
```shell
docker run --rm --name qyt-dockerfilexiao/centos_ifconfig eth0
```

### CMD
如果你自己的 Dockerfile 中不写 CMD, 就会集成基础镜像中的 CMD
CMD + 环境变量是比 Entrypoint 用的更多的, 更标准的做法
