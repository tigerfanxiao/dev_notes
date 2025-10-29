

查看 docker 细节
```shell
# 这个具体的配置文件其实是映射到宿主机的文件夹下的 
docker inspect container_id
# 使用模板来查看配置信息
docker inspect -f "{{.NetworkSettings.IPAddress}}" container_name
# view container ip
docker inspect <container_name> | grep -i ip
```

安装一个工具
https://medium.com/@gchandra/install-jg-on-centos-7-459dd650baa3

```shell
sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
sudo yum install jq -y
```


```shell

# 查看现有的容器和镜像的区别
docker diff container_id 

# 类似虚拟机打快照的功能, 生产环境用的不多. 应该用 dockerfile 来构建镜像
# -a 作者名字
docker commit -m "installed net-tools" -a 'xiaofan' container_id tigerfanxiao/centos_installed_net_tools # 最后是新的镜像的名字



```