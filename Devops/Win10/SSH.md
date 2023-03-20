
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

## 生成sshkey
ssh-keygen 