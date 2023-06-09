09
[B站视频](https://www.bilibili.com/video/BV1gr4y1U7CY?p=8&spm_id_from=pageDriver&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
### Docker 架构
Docker 是 CS 结构的, 有 client 部分和 Server 部分. client 执行docker build等命令, docker Deamon(守护进程) 收到命令后控制容器和镜像. 如果本地没有镜像, 则到 docker hub (Registry)下载



# 名词解释
* Container 我们可以把每个 container 认为是一个进程
* Bare Metal 裸金属服务器, 也就是物理机
* Docker Hub 一个在线的 docker 镜像仓库. 华为内部有自己的 docker 仓库


docker和虚拟机的区别?
1. 虚拟化技术的颗粒度是系统级别的. 容器化技术是以进程为颗粒度. 在系统颗粒度上, 很容易造成系统的闲置和浪费. 因为在基础架构上, 有 hypervisor, hypervisor 上有guest os, guest os里面才是应用. 而 docker是跑在 docker engine 里, 多个容器共享一个操作系统内核.
2. 微服务其实也可以放在虚拟机上, 不一定是 docker. 



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
# bash表示 shell
docker run -it --name <container-alias> <image name> bash

# -restart always 如果 container 挂了就重启
# -d or -detach 在后台运行, 不是交互式的
docker run -dt --name bg-container --restart always alpine
# -p 把container的端口映射到本地的 host_port
docker run -p <host_port>:<container_port> --dt --name <container> <image-name>
# -rm  在推出交互模式后, 把容器销毁
docker run -it --name <container_name>:tag --rm <image> bash 
# 推出容器
exit
```

### **restarting parameters**

-   `no` - Never restart
-   `always` - Always restart when stopped
-   `unless-stopped` - Restart unless manually stopped
-   `on-failure<:retries>` - Restart when the container fails; we can supply the maximum amount of retries

```bash
# 当 container 已经启动后, 进入交互模式
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