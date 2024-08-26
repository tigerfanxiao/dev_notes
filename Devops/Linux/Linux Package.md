debian 和 redhat (rpm based)适用不同的包管理工具
- debian 适用 apt, redhat 适用 yum
- debian 发行版的安装文件 `.deb`, rpm 的安装文件`.rpm`. 但是这两个文件格式可以相互转换

You can use alien tool to convert it
```shell
# rpm to deb
alien <package_name>.rpm

# deb to rpm
alien -r <package_name>.deb
```
# apt


```shell
# ubuntu 20.04
sudo apt-get update # find avaible package for your distro
sudo apt-get upgrade # install the available package

sudo apt-get update && sudo apt-get intall <package_name>

# 配置自动更新 
sudo dpkg-reconfigure unattended-upgrades
# 不自动更新下面这些包
sudo apt-mark hold kubelet kubeadm kubectl
# 对不能自动更新的包,手动指定更新
sudo apt-get install -y --allow-change-held-packages kubelet=1.27.2-00 kubectl=1.27.2-00
```
# yum
1. 可以直接下载二进制包, 如果执行 bash 脚本来安装
2. 可以下载代码, 在本地编译后安装
3. 可以把下载二进制包的网址添加到 yum 的远程仓库中, 自动下载安装


下面我们介绍第 3 种方法

```shell
sudo yum update 
sudo yum upgrade
sudo yum install <package_name>

# 在 CentOS 8 中安装 Terraform
# 先确认一下 yum-utils 安装工具有没有
sudo yum install -y yum-utils

# 把仓库的地址添加到 yum-config-manager 中
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
# 下面是添加成功的回显
Adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
# 安装 terraform
sudo yum install -y terraform

```


