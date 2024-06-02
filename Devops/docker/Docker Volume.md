
当我们用  docker inspect 中看到 dockerfile 中有 Volume 这个键被定义时, docker 会在host 宿主机开一个目录存储卷. 这个位置在 mountpoint 键下的 source 被定义
即使容器被删除了, 卷不会被删除

```shell
# 查看所有卷
docker volume ls 
# 删除指定卷
docker volume rm volume_id 
```

匿名挂载
很少用
只指定容器内部的目录, 不指定宿主机的目录. docker 会随机产生一个宿主机的目录
```shell
docker run -it -v /DATA/QYT --name my_centos centos8
```

具名挂载, 外面使用绝对路径. 双向可读写
```shell
docker run -it -v /root/psql:/var/lib/postgresql/data -p 5432:5432 -e POSTGRES_PASSWORD=Cisc0123 -d postgres
```

外面可读写, 容器里面是只读的`ro`
```shell
docker run -it --name qytcentos -v /test:/test:ro centos 
```


### 同步卷
两个容器使用相同的卷

```shell
docker run -it --name qytcentos-2 --volumes-from qytcentos centos 
```