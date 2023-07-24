
### 查看系统信息 

- **/bin**: Pronounced "bin," this directory is where most of the binary files are stored. Inside it, you typically find the Linux terminal commands such as cd (change directory), pwd (print working directory), and mv (move).
    
- **/boot**: Contains the files necessary for Linux to boot.
    
- **/dev**: Here you find your mounted physical devices such as your hard drives, USB drives, and optical drives.
    
- **/etc**: This directory is where the configuration files of the installed software are located.
    
- **/home**: The home directory contains a directory for each user with the user's files and directories. The figure shows the directories of the user _Cisco_.
    
- **/lib**: Contains libraries that are usually downloaded with software. They are necessary for the programs to work.
    
- **/media**: Here you might find the mounted USB drives, but it depends on the Linux distribution.
    
- **/mnt**: When mounting a device in Linux, you use the _mnt_ directory.
    
- **/opt**: Contains optional software.
    
- **/proc**: The processes directory contains files for the core operating system (kernel) to work.
    
- **/root**: For a user, the home directory is _/home/username_. For the superuser, the home directory is /root.
    
- **/sbin**: Dedicated to a command that the superuser (root) can run, similar to /bin.
    
- **/tmp**: Files that are used only temporarily are stored in this directory. The content of the directory is deleted on shutdown.
    
- **/usr**: Used for sharing files between users.
    
- **/var**: Here you find files of variable lengths, usually log files of the installed software.

### cd

```shell
cd - # 回到刚才的目录
cd # 直接回到home目录
```


```shell
# 查询 redhat 版本
cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)  # 版本为 7

uname -r  # 内核
3.10.0-1160.88.1.el7.x86_64  


```

### su

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

### Network

```shell
#  查询 ip 地址
ip a s 
```

ssh
配置本机为 ssh 客户端
```shell
ssh-keygen  # 生成 ssh 秘钥
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

