
Docker File 的难度是 Linux 的水平问题
## 镜像的分层
一个镜像是又很多层构成, 每一层称为一个 Layer. 在构建镜像的时候, 每个 `RUN`命令会创建一个 layer, 而每一个 layer 都会扩大镜像的体积, 所以一般在 `RUN` 命中放了很多用`&&`连接的命令
如果要构建一个镜像. 创建`Dockerfile` 文件
`FROM` 命令这里用的是基础镜像. 基础镜像一般也有很多层了. 新的镜像会讲 Layer 放在后面. 

* 删除镜像中的内容, 并不是真的删除. 而是叠加新的一层, 伪装成删除的样子. 这是 AUFS 的特性. 一种类似 git 所使用的文件系统. 

注意: Image 文件的每一行都等于一层
# 步骤
1. 创建`Dockerfile` 文件
2. 执行 `docker build`命令 
3. 通过镜像来创建容器

例子: 创建一个 python image
``` Dockerfile
# 使用 python 3.9 的基础镜像
FROM python:3.9-slim-buster 
WORKDIR / 
# COPY的第一个参数, 是本地文件相对于 Dockerfile 文件所在的位置
# COPY的第二个参数, 容器中文件相对于WORKDIR 目录的位置
COPY ./requirements.txt ./requirements.txt
COPY ./main.py ./main.py
# 创建 python 环境
RUN pip3 install -r requirements.txt 
CMD ["python3", "./main.py"]
```

### Build Image
- 通过本地的 dockerfile 文件来构建镜像
```shell
# image-name 是自己起的名字
# -t 表示除了镜像名之外还要加上一个版本号 tag, 比如 0.1
# . 表示docker deamon 会在本地目录下寻找 Dockerfile 文件
docker build -t xiao/image_name:0.1  .
# 在当前目录中找指定文件名的 Dockerfile 文件
docker build -t xiao/image_name:0.1 -f my_docker_file_name.Dockerfile .

# 传入环境变量 IF_NAME=lo0
docker run --rm --name qyt-dockerfile -e IF_NAME=lo0 xiao/centos_ifconfig
# 最后传入的命令, 可以把定义在 CMD 中的命令覆盖掉
docker run --rm --name qyt-dockerfilexiao/centos_ifconfig ls 

```

### Dockerfile 的命令

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

常见的 FROM 命令
```dockerfile
FROM centos:8
```

常见的 RUN 命令
``` dockerfile
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
# 如果是网络上的资源, 可以用 wget, curl 下载下来
# 如果是网络上的代卖, 可以用 git clone 下载下来

```

本地复制文件

```dockerfile
ADD test.tar.gz /qytang # 会自动解压并复制
COPY file /qytang  # 复制文件, 用的比 ADD 多
```

环境变量
```dockerfile
ENV IF_NAME=eth0 SSH_PORT=22 # 等号表示默认值
ENV <key> <value> # 必须强制要传值
```

### Volume
```dockerfile
VOLUME /qytang # 把/qytang 的中文件挂载到镜像上
```

### Entrypoint
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
