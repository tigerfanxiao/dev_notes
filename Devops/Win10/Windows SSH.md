## 在windows 上生成sshkey
```shell 
# github 使用
ssh-keygen -t rsa -C "your_email@youremail.com"
```

### 在windows中连接ssh
```shell
ssh root@hotname.com
ssh root@172.13.12.1
ssh -p 9765 root@172.18.31.1 # 如果对端的IP地址已经做了修改
```

# 问题
1. win10上本地和git bash中使用ssh agent并不是一个。 gitbash中有独立的ssh。在gitbash中使用git push调用的是gitbash中的ssh agent， 感觉理论上需要配置ssh-add 列表和打开gitbash就自动启动


在win10上安装ssh服务端
setting - Optional Feature - Add a Feature - OpenSSH

在文件浏览器的地址栏中输入
%programdata%
打开ssh folder

启动ssh服务
computer management - Services and Application - Services 
在右面的列表中找到
OpenSSH SSH Server 双击打开属性
service name sshd
change Startup type automatic 开机启动
点击start
服务启动后，在ssh folder下会生成很多启动配置文件

查找到本机的ip地址


