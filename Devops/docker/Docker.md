[[Dockerfile]]

# 名词解释
* Container 容器可以把容器理解为一种运行环境 runtime 
* Bare Metal 裸金属服务器, 也就是物理机
* Docker Hub 一个在线的 docker 镜像仓库. 华为内部有自己的 docker 仓库


docker和虚拟机的区别?
1. docker 更轻量化吧
2. 微服务其实也可以放在虚拟机上, 不一定是 docker

# 安装docker

### **centos**

```shell
sudo yum remove -y docker \\
                  docker-client \\
                  docker-client-latest \\
                  docker-common \\
                  docker-latest \\
                  docker-latest-logrotate \\
                  docker-logrotate \\
                  docker-engine


sudo yum install -y yum-utils, device-mapper-persistent-data, lvm2
# 添加仓库
sudo yum-config-manager --add-rep https://download.docker.com/linux/centos/docker-ce.repo 
# 安装
sudo yum install -y docker
# 开启 docker 服务, 启用 docker
sudo systemctl start docker && sudo systemctl enable docker
# 把cloud_user放到 docker 组里, 以后运行 docker 命令, 不需要 sudo
# 要使下面命令生效, 需要推出 shell
sudo usermod -aG docker cloud_user 
```


### **Ubuntu**

Remove existing Docker installs. update the ubuntu

```shell
sudo su
sudo apt update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository  "deb [arch=amd64] <https://download.docker.com/linux/ubuntu> $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod cloud_user -aG docker
```


# Docker 命令
### 查看容器
```shell
docker container ls # 当前在运行的
docker container ls -a  # 包含已经 stopped 的容器
docker ps -a # same as "docker container ls -a"
docker ps -aq # 不显示容器名, 只显示容器 hash code
```

### 启动容器
```shell
# -i or -interactive 交互式的
# -t or -tty 使用tty模式
# --name bg-container 是container名字
# alphine是image name
# 容器会在推出交互时终止吗?
docker run -it --name <container-alias> <image name>

# -restart always 如果 container 挂了就重启
# -d or -detach 在后台运行, 不是交互式的
docker run -dt --name bg-container --restart always alpine
# -p 把container的端口映射到本地的 host_port
docker run -p <host_port>:<container_port> --dt --name <container> <image-name>
# -rm  在推出交互模式后, 把容器销毁
docker run -it --name <container_name> --rm <image>
# 推出容器
exit
```

### **restarting parameters**

-   `no` - Never restart
-   `always` - Always restart when stopped
-   `unless-stopped` - Restart unless manually stopped
-   `on-failure<:retries>` - Restart when the container fails; we can supply the maximum amount of retries

```bash
# run command in container
docker exec <conainter_name> <command>
# interate with running container with specific shell
docker exec -it <container> <shell> # shell=/bin/bash
docker exec -it <container> <command>
# copy local file to container
docker cp <local_file> <container_name>:<container_file_path>
# copy file in container to local directory
docker cp <container_name>:<container_file_path> <local_directory>
# Stop and restart a container
docker start <container_name>
docker restart <container_name>
docker stop <container_name>
# remove container
docker rm <container_name>
docker kill <container_name>
# stop all the container
docker kill $(docker ps -aq)
# remove all stopped container
docker container prune
docker rm $(docker ps -aq)
# rename container
docker rename <old_name> <new_name>
# view all containers status
docker stats
# vieww specific container status
docker stats <container_name>
```

# Configure Container
查看docker file
```bash
# view docker file json
docker inspect <container_name>
# view container ip
docker inspect <container_name> | grep -i ip
```

# Image Management
镜像管理
```bash
docker image --help 
docker image ls # list all images download
docker images   # show local images

# remove one local image
docker image rm <image_name>

# remove all the images that no related to container
docker image prune -a
```

将一个正在运行的 container 保存为一个 image

```bash
# create image base on the container
docker commit <container_name> <image_name>
# export image as file
docker save -o <path_for_tar_file> <image_name> 
# load image file 
docker load -i <path_for_tar_file> 
```
将 image 推送的docker hub上
```bash
# push image to docker hub
docker login --username=<username>
docker tag <image_name> <username>/<repo-name>:latest # tag image
docker push <username>/<image_name> 
```

# Build Image

创建一个 nodejs 的运行环境
```dockerfile
# 初始的镜像
FROM node:10-alpine 
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
# change directory, 后面的命令都会以这个目录为基础运行
WORKDIR /home/node/app  
# Copies app package* files to the working directory
COPY package*.json ./ 
RUN npm config set registry <http://registry.npmjs.org/>
RUN npm install
# copy the all the local files to the docker working directory and set the owner to node:node
COPY --chown=node:node . .
USER node
EXPOSE 8080
# another way to run command
CMD [ "node", "index.js" ]
```

build image by docker file

```shell
# 如果本地只有一个 dockerfile 文件, 不需要特别指定
docker build . -t <image_name>  
# 特别指定 dockerfile
docker build <docker_file> -t <username>:<image_name>
```

# Docker Volume
用于挂载外部存储

# Orchestration 编排器

Kubernetes或者 AWS EKS 是 docker 的编排器

# Scenario

下面可以造点例子出来. Zero-Downtime Deployments

A zero-downtime deployment (with containers) goes like this:

1.  Spin up containers running the new code.
2.  When they are fully up, direct user traffic to the new containers.
3.  Remove the old containers running the old code. No downtime for users!

Containers are great at accomplishing things like:

-   Software Portability – Running software consistently on different machines.
-   Isolation – Keeping individual pieces of software separate from one another.
-   Scaling – Increasing or decreasing resources allocated to software as needed.
-   Automation – Automating processes to save time and money.
-   Efficient Resource Usage – Containers use resources efficiently, which saves money