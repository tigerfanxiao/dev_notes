

```bash
# 查看所有的镜像 ID
docker images -aq


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