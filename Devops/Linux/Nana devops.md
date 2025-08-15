Ubuntu 22.04
```shell
# check Ubuntu version
lsb_release -a
```
 root 用户切换
```shell
# switch to root without password
sudo su 
# switch to root with password of current user
sudo -i

exit # exit root 
```

# File system
Linux 所有的文件都在根目录下. 是一个树形结构
- 家目录. root 用户的家目录在 `/root`下, 其他用户的家目录在 `/home/`下. 不同用户的家目录是相互隔离的
- `/bin` 和 `/sbin` 存放的二进制的可执行程序, 编译好的. 区别在于, 一般`/bin`内程序是普通用户就可以执行的, 用户和系统交互时使用的. `/sbin`则多为系统自己调用使用的, 或者高危的需要 root 用户才能执行的程序

```shell
# /bin 下的程序
echo
apt
cat
copy
# /sbin 下的程序
adduser
password
iptable
```
- `/lib`, `/lib32`, `/lib64`, `libx32` 这些都是 library. 是`/bin`和`/sbin`下的可执行程序的依赖
- `/usr/` 这个目录下一套 `/bin`, `/sbin`, `/lib`, 这些也是系统层面可以调用的命令, 因为历史原因, 系统层面可以调用的命令被分在两个地方. `/usr/local/`下面又有一套 `/bin`, `/sbin`, `/lib`, 这里放的是用户自己在使用过程中安装的应用, 比如 java, docker, minikube, python. 注意安装这里的程序, 所有人都可以使用的, 不是用户独享的
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
