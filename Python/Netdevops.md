
python 中使用网络相关的模块
Kamene 可以用来构建各种网路流量

netmiko/paramiko 用来做 ssh
request 用来做 http 请求
Python 版本太新的话， 很多第三方模块跑不了。 3.10 可以

rockyos 其实是 centos 8 

```shell
yum install -y open-vm-tools net-tools vim wget tcpdump bind-utils dos2unix

# 关闭 selinux
vim /etc/selinux/config
SELINUX=disble

# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

yun update
yum install -y gcc python3.10 python310-devel
ln -sb /usr/bin/python3.10 /usr/bin/python3
ln -sb /usr/bin/pip3.8 /usr/bin/pip3


```
