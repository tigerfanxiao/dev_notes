- ` docker run` 本质上是创建的一个容器, 不是启动一个容器. 一般在调试的时候用. 在生产中拉容器, 一般是用 docker compose 文件


常见 docker run 的命令文档
https://docs.docker.com/engine/reference/commandline/run



```shell

docker run --name my_docker_name centos # 基于 centos 镜像启动并命名一个容器
docker run --name mydocker_name -d centos  # 后台运行 
docker run --name mydocker_name -d centos /bin/bash # 后台运行, 并运行 /bin/bash 命令

-it # 交换界面
-p ip:主机端口: 容器端口
-P # 随机指定端口

docker ps # 显示正在运行的容器
docker ps -a # 显示所有容器, 包括运行停止的

docker kill container_id # 终止某个正在运行的容器

# 进入某个正在运行的 container 观察
# 使用 ctrl + p, ctrl + q 退出 attach 的状态
docker attach container_id 
# 进入容器的 vty 线路, 交互式界面 /bin/bash
docker exec -it container_id /bin/bash 


```


### Best Practice
- 容器的正常做法, 是要用一个程序定在 console 口里定在前台运行的. 所以一般也不是用 attach 进去操作容器, 因为主进程顶住了 console 口, 操作不了. 如果进入容器操作, 需要使用vty
- 尽量使用`-d`, 否则一个一直在运行的程序会顶住你的 console 口运行. 你没法操作, 用 ctrl + C 也退不出来
- 可以使用 `docker attach container_name` 进入某个正在后台运行的容器