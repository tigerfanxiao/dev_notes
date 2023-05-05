# 问题
我怎么查看基础镜像的内容?


## 镜像的分层
一个镜像是又很多层构成, 每一层称为一个 Layer. 在构建镜像的时候, 每个 `RUN`命令会创建一个 layer, 而每一个 layer 都会扩大镜像的体积, 所以一般在 `RUN` 命中放了很多用`&&`连接的命令
如果要构建一个镜像. 创建`Dockerfile` 文件
`FROM` 命令这里用的是基础镜像. 基础镜像一般也有很多层了. 新的镜像会讲 Layer 放在后面. 

* 删除镜像中的内容, 并不是真的删除. 而是叠加新的一层, 伪装成删除的样子. 这是 AUFS 的特性. 一种类似 git 所使用的文件系统. 


# 步骤
1. 创建`Dockerfile` 文件
2. 执行 `docker build`命令 
3. 通过镜像来创建容器

例子: 创建一个 python image
``` dockerfile
FROM python:3.9-slim-buster
# COPY的第一个参数, 是本地文件相对于 Dockerfile 文件所在的位置
# COPY的第二个参数, 容器中文件相对于WORKDIR 目录的位置
COPY ./requirements.txt ./requirements.txt
COPY ./main.py ./main.py
RUN pip3 install -r requirements.txt
CMD ["python3", "./main.py"]
```

### Build Image
```bash
# image-name 是自己起的名字
# -t 表示除了镜像名之外还要加上一个版本号 tag, 比如 0.1
# . 表示docker deamon 会在本地目录下寻找 Dockerfile 文件
docker build -t image_name:0.1  .
docker images # 查看本地镜像列表, 一般会看到基础镜像, 和基于基础镜像构建的新镜像

# 查看镜像的大小. 注意使用
docker inspect -f "{{ .Size }}" widgetfactory:0.1
# 查看 layer
docker inspect -f "{{ range .RootFS.Layers }}{{ println .}}{{ end }}" widgetfactory:0.1
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
