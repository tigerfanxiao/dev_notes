### debian based
- debian
- ubuntu
- mint

RPM based
- Red hat
- centos
- fedora
- opensuse

Arch 非常小的 linux 版本, 极高的定制化, 多用于 docker 镜像


### 文件系统
    
- **/dev**: Here you find your mounted physical devices such as your hard drives, USB drives, and optical drives.
    
    
    
- **/lib**: Contains libraries that are usually downloaded with software. They are necessary for the programs to work.
    
- **/mnt**: When mounting a device in Linux, you use the _mnt_ directory
    
- **/opt**: Contains optional software.
    
- **/proc**: The processes directory contains files for the core operating system (kernel) to work.
    
- **/root**: For a user, the home directory is _/home/username_. For the superuser, the home directory is /root.

    
- **/tmp**: Files that are used only temporarily are stored in this directory. The content of the directory is deleted on shutdown.
    
- **/var**: Here you find files of variable lengths, usually log files of the installed software.

### 切换目录cd

```shell
cd - # 回到刚才的目录
cd # 直接回到home目录
```

### 查看系统信息 
```shell
# 查询 redhat 版本
cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)  # 版本为 7

# 查看内核版本
uname -r  
3.10.0-1160.88.1.el7.x86_64  

# 查看环境变量
env

# 打印变量
a=test
cisco@cisco:~$ echo $a

```

# 文件

### 创建文件夹
```shell
mkdir -p ./a/b/c # 会把子目录一起创建出来
```

### 复制文件

```shell
# 复制文件到指定目录下
cp file folder
```


### 用户权限 su
`#` 表示当前用户为 root 账户
`$` 表示当前用户是普通用户


su 是用来切换成 root 账户
sudo 不切换账户, 但是可以用 root 的权限
注意: 如果要运行某个 sudo 命令, 要先确保当前用户在 `sudoers` 文件中配置
```shell
# 查询userid, groupid
id

# 下面两个命令需要知道 root 用户的密码
su - root # 切换到 root 用户
logout # 退出 root 用户
su - # 默认切换到 root 用户
su - cloud_user # 切换到 cloud_user

# 下面这个命令, 假设当前用户是有 sudo 权限的, 但是不需要 root 的密码, 只需要当前用户的密码
sudo -i # swith to root

sudo dnf update # 如果 root 一样运行 dnf update 命令
```

### 包管理

#### YUM
yum是一个包管理工具， 安装包是 `.rpm` 结尾的
```shell
# 如果已经连接了互联网
sudo yum install tracertoute
# 如果没有连接互联网， 安装的包已经下载到本地
sudo rpm -i <pckage-name>.rpm
```

### DPKG
```shell
sudo apt-get install traceroute # 下载并安装


sudo apt install traceroute # 下载并安装
sudo dpkg -i <package-name>.deb  # 本地安装
```

### Network

```shell
#  查询 ip 地址
ip a s 

# 查看状网卡接口状态
nmcli

# 查看网卡信息, 是否会开机启动
cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
# 把 onboot 改为 yes

# 重启网卡
systemctl restart network.service


```

ssh
配置本机为 ssh 客户端
```shell

ssh-copy-id <SECOND_PUBLIC_IP_ADDRESS> # 将本机的公钥发给 ssh 服务端

# add cloud_user identity to the agent and to reload the agent
eval $(ssh-agent -s)
```

连接
```shell
# 两种登录方式都能用
ssh -l username 1.1.1.1 # if username has some special symbol like @
ssh username@1.1.1.1 

```


```shell
ssh -t cloud_user@<SECOND_PUBLIC_IP_ADDRESS> df -hT >> server_health.txt

ssh -t cloud_user@<SECOND_PUBLIC_IP_ADDRESS> free >> server_health.txt
```

问题
echo export = .bash_profile 是什么意识?用冒号分割是什么意思

# Bash
Bourne Again Shell
Bash 的 script 要用 `#!`开头, 表示调用`/bin/bash`来运行这个脚本

### 执行 bash 脚本
```bash
# 先检查脚本文件有没有执行权限
chmod +x bashfile.sh 
# 执行脚本
sh bashfile.sh  
```


# 进程
什么是守护进程

```shell
top  # 查询当前 CPU 占用, 每个进程对进程的占用, 是 realtime实时的信息, 所以清单会变动, 更具 CPU 占用的比例排序. 当然, 你可以指定排序为 CPU 占用, 内存占用或者 runtime
htop # human easily read 允许下滑表单,杀死进程

ps   # 查询当前正在活动的进程, 与 top 相比可以接受更多的参数
ps aux | grep firefox  # 查询所有的进程
ps aux -sort=-pcpu,+pmem # 根据cpu 占用和内存占用排序

kill <pid>  # 杀死一个进程
killall firefox # 杀死 firefox 这个进程的所有实例
pkill # 按进程名删除进程


```




