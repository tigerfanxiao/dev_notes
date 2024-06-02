
默认容器是属于 172.17.0.0/16 网段. 宿主机的地址是 172.17.0.1 . 一般是现实是 docker0这个网口

默认情况下, 所有的容器都会连接到 bridge 这个网络 
在宿主机上多创建一个容器, 宿主机上就会多一个 veth 的接口
容器默认使用宿主机的 dns 解析配置

```shell
docker inspect -f '{{ .NetworkSettings.IPAddress }}' <container_name>
# 
docker network ls

docker inspect <newwork_id> 

# 查看 veth 接口和 bridge 是怎么连接的
bridge link 
# 看看端口映射信息 
docker port <container_name> 

```


```shell
# 进入容器, 然后 ctrl p,q 退出来, 容器没有退出
docker run -itd --name docker-net1 xiao/centos-ifconfig /bin/bash 
```


```shell
wget https://https://raw.githubusercontent.com/micahculpepper/dockerveth/master/dockerveth.sh
chmod + x dockerveth.sh

./dockerveth.sh

cat /sys/class/net/vethcca2f5d/ifindex
docker exec -it docker_net1 cat /sys/class/net/eth0/iflink


```


```shell
docker run -itd --name docker-net1 -p 8080:80 xiao/centos-ifconfig python -m SimpleHTTPServer 80 # 用 python 开一个 80 端口的服务器

```
如果宿主机有多个 IP 地址, 可以指定 IP 地址

docker默认的网络有问题, 就是要通过 IP 地址访问
一般情况下, 如果要容器要互相访问, 都通过主机名来访问

下面这种 link 的方法不常用

```shell
# 等于是在 docker-net1 中的 /etc/hosts 文件中, 给 docker-net2 做了一个主机映射. 但是这是一个单向的映射. 
docker run -itd --name docker-net1 --link docker-net2 xiao/centos-config /bin/bash 
```


# 自定义网络
- 使用自定义网络, 可以直接用主机名相互访问
- 默认自定义网络和默认网络是隔离的
-  /etc/hosts 是没有做的, /etc/resolv.conf dns 配置变了. 只要是自定义网络, 会做一个嵌入式的 dns 服务 127.0.0.11. 解析这个自定义网络中的所有的主机名

```shell
# 创建一个 bridge 类型的网络
docker network create -d bridge gyt-net 
# 可选配置子网和网关qyt-net1
docker network create -d bridge --subnet 172.22.16.0/24 --gateway 172.22.16.1 
# 还可以宿主机的某个接口
-o parent=ens192.5

# 查看所有的 bridge 网络
brctl show 
bridge link # 在 centos8 中使用

docker run -itd --name docker-net3 --netowrk qyt-net1 xiao/centos /bin/bash
```

安装 nslookup
```shell
yum install -y bind-utils

nslookup docker-net3
```


一个容器也可以接入到两个网络中

```shell
# 将容器接入到 qyt-net1 的网络中 
docker network connect qyt-net1 <container_name>
```

# host网络
相当于容器就直接使用宿主机的网络, IP 地址和宿主机一样. 这种情况下, 容器的端口号和宿主机容器起冲突
```shell
docker run -it --net=host --name qyt-centos-1 xiao/centos-config /bin/bash
```

# None网络
没有网络, 只有自己环回口

# MACVLAN 网络(Trunk)
平时很少用

