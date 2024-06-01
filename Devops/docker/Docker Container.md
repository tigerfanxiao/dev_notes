一般在调试的时候用 docker run
而拉容器, 一般是用 docker compose 文件
https://docs.docker.com/engine/reference/commandline/run

```shell

docker run 
-name="Name" # 容器的名称
-d # 后台方式运行
-it # 交换界面
-p ip:主机端口: 容器端口
-P # 随机指定端口


docker ps -a # 显示所有容器, 包括运行停止的
```