Ubuntu 22.04

```shell
# check Ubuntu version
lsb_release -a
```
 root 用户切换
```shell
# switch to root without password
sudo su 
exit # exit root 

sudo -i  # switch to root with password of current user
```
# Package Management

```shell
# ubuntu 20.04
sudo apt-get update # find avaible package for your distro
sudo apt-get upgrade # install the available package

sudo apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/list*

sudo apt-get update && sudo apt-get intall <package_name>

# 配置自动更新 
sudo dpkg-reconfigure unattended-upgrades
# 不自动更新下面这些包
sudo apt-mark hold kubelet kubeadm kubectl
# 对不能自动更新的包,手动指定更新
sudo apt-get install -y --allow-change-held-packages kubelet=1.27.2-00 kubectl=1.27.2-00
```
