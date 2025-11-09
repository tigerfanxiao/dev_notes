


常见 docker run 的命令文档
https://docs.docker.com/engine/reference/commandline/run


```shell

docker run --name <container_name> <image> # 基于 centos 镜像启动并命名一个容器
docker run --name mydocker_name -d centos  # 后台运行 
docker run --name mydocker_name -d centos /bin/bash # 后台运行, 并运行 /bin/bash 命令
docker run --rm --name mydocker_name -d centos /bin/bash # 容器退出了, 容器会自动删除. 

-it # 交换界面
-p ip:主机端口: 容器端口


docker ps # 显示正在运行的容器
docker ps -a # 显示所有容器, 包括运行停止的
docker ps -aq # 所有容器的编号

docker kill container_id # 强行终止某个正在运行的容器, 并不删除. 类似 power off, 拔电
docker rm container_id # 删除已经终止的容器
docker rm -f container_id # 强制删除正在运行的容器
docker rm -f $(docker ps -aq) # 删除所有容器

# 进入某个正在运行的 container 观察
# 使用 ctrl + p, ctrl + q 退出 attach 的状态
# 注意不能用 ctrl + c, 因为会直接终止主进程
# docker attach 进去是 tty 是 0, exec 进去的时候 tty 是 1
docker attach container_id 
# 进入容器的 vty 线路, 交互式界面 /bin/bash
# 同样是使用 ctrl + p, ctrl + q 退出
docker exec -it container_id /bin/bash 
# 如果只是想看一下前台运行的日志, 不是进入容器
docker logs container_id # 查看容器日志
docker logs -tf container_id # 跟随日志, -t 表示timestamp, -f 表示跟随

docker stop container_id # 正常关闭容器
docker start container_id # 开启容器
docker restart container_id # 重启
docker pause container_id # 暂停容器
docker unpause container_id # 恢复容器

```

把文件 copy 到正在运行的容器中
- 这种文件 copy 的工作是在 `dockerfile` 中操作
```python
# 把 host 中的文件 copy 到容器中
docker cp ./t.sh container_id:/t.sh

# 把容器中文件 copy 到本地中
docker cp container_id:/test.txt text.txt
```
### Best Practice
- 容器的正常做法, 是要用一个程序定在 console 口里定在前台运行的. 所以一般也不是用 attach 进去操作容器, 因为主进程顶住了 console 口, 操作不了. 如果进入容器操作, 需要使用vty
- 尽量使用`-d`, 否则一个一直在运行的程序会顶住你的 console 口运行. 你没法操作, 用 ctrl + C 也退不出来
- 可以使用 `docker attach container_name` 进入某个正在后台运行的容器
- ctrl + p, ctrl + q 其实是把程序挂起, 不是退出容器. 如果在容器内运行 exit, 则退出程序时, 往往容器也退出关闭了
# 资源限制

```shell
# cpu资源测试工具
yum install -y epel-release
# 内存压力测试工具
yum install -y stress

# 不断关联 128m 内存
stress --vm 1 --vm-bytes 128m -v
stress --vm 1 --vm-bytes 16000m -v # 关联16G内存, 如果物理内存只有8G, 但是可以用置换的方法增加到 16G 内存. 但是一般是比 16G 小的. 所有用 15000m 应该可以运行的

# 这个命令会消耗完一个 cpu 的资源. 
stress --cpu 1

```
指定容器的内存
```shell

docker run -it --name qytang-stress --memory=200M centos8
# 在里面跑 stress 390M 可以成功弄, 但是跑 400 兆会失败

# 指定 cpu 某一个核心
docker run -it --name qytang-cpu --cpuset-cpus="0" --cpu-shares=100 xiao/centos8

```

