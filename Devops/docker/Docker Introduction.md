 09
[B站视频](https://www.bilibili.com/video/BV1gr4y1U7CY?p=8&spm_id_from=pageDriver&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
 ### Docker 架构
Docker 是 CS 结构的, 有 client 部分和 Server 部分. client 执行docker build等命令, docker Daemon(守护进程) 收到命令后控制容器和镜像. 如果本地没有镜像, 则到 docker hub (Registry)下载

注意: 容器更适合执行单进程, 如果要做多进程. 适合拉多个容器, 做负载均衡

# 名词解释
* Container 我们可以把每个 container 认为是一个进程
* Docker Hub 一个在线的 docker 镜像仓库. 华为内部有自己的 docker 仓库


### Docker和虚拟机的区别?
虚拟化的颗粒度不同
1. 虚拟化技术的颗粒度是系统级别的. 容器化技术是以进程为颗粒度. 简单地理解就是多个 docker 共享一个操作系统内核, 特别是 linux 尽管有这么多不同的版本, 但是 linux 内核是各种版本共用的. 但是类似 Vmware 这样的虚拟机, 每一个虚拟机就有独立的操作系统内核, 说一容易造成资源限制
2. Docker 的构建是由 Docker File 完成的, 构建的过程可以被描述. 虚拟机则是原始装了一个系统后, 在系统内部做各种不可描述的操作
3. Docker 的使用方法, 往往是需要的时候启动一个, 不需要的时候, 则废弃. 而不是进入 docker 内部去操作. 进入 docker 内部更多是为了排障. 虚拟的使用方法往往是进入虚拟机内部去操作
4. 容器一种服务, 理论上说, 把服务跑完了, 容器就停了. 但是虚拟机一般是一直开着
5. 服务会随着主进程的奔溃而奔溃. 但是虚拟机不会因为服务奔溃而奔溃 
6. 容器一般是不需要登录的, 而是编辑 docker file. 虚拟机一般是登录后操作的
7. 容器一般是要让一个程序在 console 口顶着运行, 而一般虚拟机是中的 web 服务是在后台运行的
8. 容器适合做单进程, 且所在前台顶着 console 口. 可以利用 docker swam 或者 k8s 做负载. 虚拟机则是运行多进程


### 微服务
1. 微服务其实也可以放在虚拟机上, 不一定是 docker. 

### Docker Operation
1. 创建 Dockerfile 文件
2. 用 Docker File 文件构建镜像
3. Docker Run 镜像


# Docker 命令

[命令参考文档](https://docs.docker.com/reference/cli/docker/container/run/)

### 镜像相关命令

在 docker hub 上搜索官方镜像

```shell
docker pull centos:8 # tag 指定了版本. 如果不填就是最新的

# 查看所有 centos 的镜像, 所有不同的版本
docker images centos 

docker images -aq # 所有 image 的 ID
docker images --digest # 查看image的hash值
docker rmi centos:7 # 删除镜像
docker rmi 590dd # 根据简写的 ID 来删除镜像
docker rmi 590d 4540d # 删除多个镜像
docker rmi -f $(docker images -aq) # 删除所有镜像
  
```
注意:
1. tag 不同, 但是镜像可能是相同的. 可以通过hash 值来判断两个镜像是否相同
2. 不同镜像的有些层是一样的, docker 不会重复拉. 所以两个镜像真正占据的磁盘空间是小于纸面上两个镜像大小的和
### 容器相关命令 
docker run 的命令一般用在调试阶段. 调试结束后更多地使用 docker compose 用 yaml 文件来拉起容器. 且多个容器可以一起启动

```shell
docker ps # 查看正在运行的容器
docker ps -a # same as "docker container ls -a"
docker ps -aq # 不显示容器名, 只显示容器 hash code

```

注意: docker run 只负责运行 docker 中的命令CMD, 如果命令运行完了, 就会整个 container 一起退出. 所以正常容器需要一个程序顶在 console 口运行, 这个程序挂了, 容器就挂了

```shell
 # 交互式运行
docker run -it --name <container-alias> <image name> bash
# bash表示 shell
# -i or -interactive 交互式的
# -t or -vty 使用vty模式

# 后台运行
docker run -d --name <container-alias> --restart always <image name>
# -restart always 如果 container 挂了就重启
# -d or -detach 在后台运行, 不是交互式的

# -p 把container的端口映射到本地的 host_port
docker run -p <host_port>:<container_port> --d --name <container> <image-name>

# -rm  在推出交互模式后, 把容器销毁
docker run -it --name <container_name>:tag --rm <image> bash 


# 强制关闭容器, 类似拔电源
docker kill <container_id>
# 正常关机
docker stop <container_id>
# 从中断中恢复
docker start <container_id>
# 重启 
docker restart <container_id>

# 进入容器的console, 一般情况下是被主进程锁死的. 不建议使用, 容易被主进程顶死
docker attach <container_name> 
# 进入容器的 vty 链路, 进到交互式界面做操作. 不会影响到主进程, 但是可以做修改
docker exec -it <container_id> /bin/bash 
docker exec -it <container_id> ip add # 直接执行 ip add 命令返回结果

# 如果在 docker 的交互界面里
ctrl + p,ctrl + q  # 可以挂起退出
docker attach <container_name> # 重新进入交互模式

# 查看容器的日志, 不用进入 console 口查看运行状态
docker logs <container id> 
docker logs -tf <container id > # 打印日志不停



# 删除已经停止的容器, 一般情况下无法直接删除一个正在运行的容器
docker rm <container_id> 

# 强行删除一个正在运行的容器
docker rm -f <container_id>

# 删除所有的容器
docker rm -f $(ps -aq)

# 当 container 已经启动后, 进入交互模式
docker exec -it <container> <shell> # shell=/bin/bash
docker exec -it <container> <command>

# copy local file to container
docker cp <local_file> <container_name>:<container_file_path>
# copy file from container to local directory
docker cp <container_name>:<container_file_path> 
<local_directory>

docker container prune
# rename container
docker rename <old_name> <new_name>

# view all containers status
docker stats
# vieww specific container status
docker status <container_name>


```

### `docker restart` parameters

-   `no` - Never restart
-   `always` - Always restart when stopped
-   `unless-stopped` - Restart unless manually stopped
-   `on-failure<:retries>` - Restart when the container fails; we can supply the maximum amount of retries



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