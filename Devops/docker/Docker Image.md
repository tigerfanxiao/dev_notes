
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
``` dockerfile
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
docker build -t image_name:0.1  .

```



```bash

docker images # 查看本地镜像列表, 一般会看到基础镜像, 和基于基础镜像构建的新镜像

# 查看所有的镜像 ID
docker images -aq

# 通过镜像名来删除镜像
docker rmi centos
# 查看 images 的 hash
docker images --digest
# 删除所有的image, 包括正在运行容器的镜像
docker rmi -f $(docker images -aq)

# 查看镜像的大小. 注意使用
docker inspect -f "{{ .Size }}" image_name:0.1
# 查看 layer
docker inspect -f "{{ range .RootFS.Layers }}{{ println .}}{{ end }}" image_name:0.1
```

# create container
```bash
docker run <image-name>
```

# image CMD

```Dockerfile
USER node
EXPOSE 8080
# another way to run command
CMD [ "node", "index.js" ]
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

# 修改镜像的名字, 把镜像推到私有仓库的位置
docker tag docker_hub_account/centos_old private_hub/centos_new
```

保存容器     
```shell
# 把容器的保存下来, 而不是镜像 
docker export -o qytcentos-1.tar container_name

# tar文件引入作为一个镜像
docker import qytcoentos-1.tar xiao/qytcentos-1

# 注意: 这时导入的镜像可能主命令都没有了
docker run -it --name qytcentos-7 xiao/import_image /bin/bash # 增加一个主命令
```

注意:  建议使用 save 和 load, 不要用 export 和 import. 因为 export 里面没有层的信息. 镜像中有层的信息, 有层的信息, 可以节约资源. 不用下载全部的层
```bash
# create image base on the container
docker commit <container_name> <image_name>
# export image 镜像 as file 
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

从镜像拉起容器
```shell
# 两个端口映射, 宿主机端口:容器的端口
docker run -it --name qytcentos-2 -p 8000:80 -p 1022:22 xiao/centos_net_tools_vim


# 查看端口映射
docker port container_name 
# 查看容器的 22 端口被映射到宿主机的什么端口上了
docker port container_name 22/tcp
```


## 上传镜像

```shell

docker login in private.dockerhub.com 
docker push xiao/centos8
```