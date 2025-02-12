
# Linux SSH

### Install openssh
在服务器侧安装服务端
```shell
sudo yum -y install openssh-server openssh-clients
sudo systemctl start sshd
```

远程登录服务器
注意: Ubunut 不允许root账户远程登录, 需要用别的账户
```shell
ssh username@<ip>
```

使用公钥文件登录ssh
```bash
chown 400 perm
ssh usernanme@ip_addriess -i perm
```

### 查看当前连接用户
```shell
# 查看当前连接
tty
# 查看所有连接
who
# 查看所有连接和运行的命令
w
```

# root
```shell
sudo -i # 切换root
```
### UID & GID
Linux的用户分User和Group
Linux给每一个用户都会分配一个ID， 也成为UID
UID的范围 
0 是superuser root用户
1-200 是系统进程
201-999 没有任何文件的系统进程
1000+ 普通用户

对于Group来说也有GID的概念. 每个用户属于一个primary group和多个secondary group. 如果创建一个新的用户, 会同时给这个用户创建一个同名的group, 然后把这个用户的primary group配置成这个组

```shell
# find current user name
whoami
# find current user name and his ip address
who am i
# find uid of current user
id <username>
# find principle and secondary groups of user
groups <username>
```

### Files for user and group
- `/etc/passwd`
- `/etc/shadow`
- `/etc/group`
- `/etc/gshadow`

#### `/etc/passwd`
- list all the users
- indicate the primary group of each user
在 `/etc/passwd` 保存这UID的信息. 按照用户被创建的顺序来保存. 且所有用户都可以查看
```shell
[cloud_user@a130ce37501c ~]$ cat /etc/passwd
# 用户名:密码:UID:GID:用户说明:家目录:shell 
root:x:0:0:root:/root:/bin/bash # root 用户的UID是0
bin:x:1:1:bin:/bin:/sbin/nologin
cloud_user:x:1001:1001::/home/cloud_user:/bin/bash # 普通用户的UID和GID, 家目录, shell
```
用户的密码保存在`/etc/shadow` 中, 并被加密了. 只有root用户可以查看
```shell
# 用户名:密码:上一次修改密码的天数(1970年开始计算):密码最短有效期:密码最长有效期:密码过期提前告警天数:密码过期后多少天, 账户被disable:账户disable之后多少天, 账户不能使用
root:$6$4D9ck/vdfDM/$hWiTXVbOh9M8dfCcWkpWApZpNqaw9EArXehbVIIr77KPOdLeGmrGi2vZx/90gsXrDqB3jEAcH4hpyIInKye3U/:20083:0:99999:7:::
cloud_user:$6$bvk/ov5SslrdY6iI$49s5sqRWaci60NNfrf5C9kT.0RzLkM5/UYAQZdeKhK8t6P5b2Tx.5lucT5XX.JsOdbzW7Cl6D8Da2Sd1ffXhj/:20117:0:99999:7:::
```
组的信息, 则保存在 `/etc/group`
```shell
# 组名: 密码:GID:组员
root:x:0:
cloud_user:x:1001:
```
组的密码保存在 `/etc/gshadow`
- `!` 表示用户不能通过`ugroup` 命令来加入这个组
- `!!` 表示用户不能通过`ugroup` 命令来加入这个组, 且没有配置组密码
```shell
# 一般情况下没有设置组的密码
# 组名:组密码:: 组管理员名单:组员清单
root:::
cloud_user:!::
```

### User Shell environment
- Bourne Shell `/bin/sh`
- Korn Shell `/bin/ksh`
- C shell `/bin/csh`
- The Bourne-Again Shell `/bin/bash`

对于系统进程用户, 配置的shell是 `/sbin/nologin` 表示如果用该用户名来登录shell会打印`/etc/nologin.txt` 并退出
注意: 如果存在`/etc/nologin`这个文件, 这表明只有root用户可以ssh登录
如果把 shell 设置成 `/bin/false` 则这个用户被会被立马退出

### Home Directory
普通用户的home directory都是在`/home/username` 下的
但是root的home directory都是在根目录下`/root`

```shell
df -h 
# 查看磁盘分区, 家目录都是在主分区下的
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        836M     0  836M   0% /dev
tmpfs           893M     0  893M   0% /dev/shm
tmpfs           893M   18M  876M   2% /run
tmpfs           893M     0  893M   0% /sys/fs/cgroup
/dev/nvme0n1p2   10G  6.5G  3.6G  65% /
tmpfs           179M  4.0K  179M   1% /run/user/1001

```

回到家目录的方法
```shell
cd 
cd ~
cd $HOME
```

Create User
`/etc/default/useradd`
```shell
# 会自动创建一个和用户同名的家目录 在/home下
useradd <username> 
# 同时会在家目录下创建 .bash_logout .bash_profile .bashrc 三个文件
# 如果要制定家目录位置
useradd -d </userb> userb # 在根目录下创建 /userb 作为家目录
# 修改用户的家目录
usermod -d </userb> userb
mkdir /userb # 新建家目录
chown userb:userb /userb # 修改家目录的所有权
chmod 700 /userb # 给userb所有的权限

# -m 表示需要make homedir
useradd -u <uid> -c "<comments>" -d <home_dir> -m -g <primary_group> -G <secondary_group> -p <password> -s /bin/sh <username>

# 修改用户信息
usermod -c "comments" -d /jane -g appadm -p <passwrod> -s /bin/sh <username>
usermod -G <groupname> <username> # configure and replace secondary group
usermod -aG <groupname> <username> # add secondary group
usermod -d /home/appteam -m <username> # move the current home directory to new directory /home/appteam
```


Create group
默认情况下都会创建 adm和wheel group. Wheel group is generally used for control access for sudo users

默认的组
adm
wheel GID 10

`/etc/login.defs` 这里定义了GID的最大最小值等信息
```shell
groupadd <groupname>
groupadd -g <GID> groupname


# modify group
groupmod -g <DID> -n oldgroupname newgroupname
# remove group
groupdel <groupname>
```
#### 家目录中的文件
- `.bash_logout` 退出shell时会执行的命令
- `.bash_profile` 第一次登录shell的时候, 会执行的命令和变量. 该用户新建一个shell的时候, 不会被执行
- `.bashrc` 用户每次新建一个shell的时候, 都会被执行
在 `/etc/skel`下定义了每个新用户的创建在家目录中会创建默认文件. 
```shell
[cloud_user@a130ce37501c ~]$ cd /etc/skel/
[cloud_user@a130ce37501c skel]$ ls
[cloud_user@a130ce37501c skel]$ ls -al
total 24
drwxr-xr-x.   3 root root   78 Jun 22  2021 .
drwxr-xr-x. 145 root root 8192 Jan 29 00:01 ..
-rw-r--r--.   1 root root   18 Jul 27  2021 .bash_logout
-rw-r--r--.   1 root root  141 Jul 27  2021 .bash_profile
-rw-r--r--.   1 root root  376 Jul 27  2021 .bashrc
drwxr-xr-x.   4 root root   39 Aug 27  2020 .mozilla   
```

```shell
rm -R /userb # 删除家目录及下面所有文件
chown userb:userb .bash* # 修改所有.bash开头的文件的用户权限和组权限
```

### Remove user

```shell
# 1. 删除和用户有关的进程
ps U <username> 
kill <PID>
# 2. 查询所有和user有关的文件
find / -user <username>
# 将需要保留的文件和目录通过chown转移给别的用户
# /var/spool/mail 会在userdel后自动删除
# 3. 删除家目录
userdel -r <username>

```
### Change user expiration time

```shell
# 修改用户过期时间为永不过期
chage -E -1 user2
```


其他工具
### `pwconv` 重新生成shadow文件
如果发现用户在`/etc/passwd` 中但是不在 `/etc/shadow` 中， 可以适用 `pwconv` 生成shadow文件, 然后需要使用`passwd` 设置密码


pwunconv
grpconv grpunconv


# visudo
直接修改 `/etc/sudoers`
把配置文件放入`/etc/sudoers.d`

### 给用户增加sudo权限
新增一个用户并给与sudo权限
检查wheel 组的权限
```shell
%wheel ALL=(ALL)    ALL # 需要密码
```
新建用户userA
```shell
useradd -G wheel userA
passwd userA # 设置新密码
sudo - userA # 切换到该用户
sudo whoammi # 应该显示root
```

# Centos 7 破解root账户密码

3. 重启设备 按e, 进入内核参数修改模式
![[Pasted image 20250131073810.png]]
4. 内核参数修改, 找到linux16 所在的段, 把`ro` 改成`rw`, 在后面加上 `init=/bin/sh`. 用户ctrl + x 进入单用户模式
![[Pasted image 20250131073822.png]]
5. 使用 `passwd` 修改root账户的新密码
6. `touch /.autorelabel` 创建表现文件, 是selinux允许对root密码的修改
7. `exec /sbin/init` 重启


# File Management
```shell
# 制定用户或者群做相同的配置
# 增加用户的权限为
chmod u+x <filename>
# 把用户的权限配置为rw
chmod u=rw <filename>
# 把群和其他用户的可读权限去掉
chmod go-r <filename>
# 给用户， 群， 其他用户的权限都配置为 rwx
chmod a=rwx <filename>

# 制定用户和群各自不同的配置
# 分别置顶用户的权限是7，群的权限是5， 其他用户的权限是0
chmod 750 <filename> 

# 修改目录的权限
chmod -R 777 <filename>
```

### Special Permission
- suid 确保执行某个文件的是文件的owner， 而不是当前的用户
- sguid
- sticky bit  `t` 用于目录. 如果用户用这个目录的写权限， 该用户或者root只能删除和重命名他自己创建的文件
```shell

chmod u+s <filename>
chmod g+s <filename>
# 如果group有执行权限， 则显示s， 如果没有执行权限则显示S
rwxr-Sr-x 1 test test 54256 Jul 5 15:26 /tmp/<filename>
rwxr-sr-x 1 test test 54256 Jul 5 15:26 /tmp/<filename>

# 如果sticky bit配置好， 会出现t标识
chmod o+t 
drwxrwxrwt 2 user group 4096 Feb 5 12:34 /tmp
```

### File ACL
当给文件设置ACL之后， 会出现 `+` 标识

```shell
getfacl <filename>

# m for modify
setfacl -m mask:rw <filename>
# 即使testuser1所在的群有权限， 但是去除testuser1对该文件的所有权限
setfacl -m u:testuser1:- testfile
# 删除所有在文件testfile1上的关于group test的ACL
setfacl -x g:test testfile1
```


# 命令提示符
区分
```shell
# # 管理员
$ # 普通用户
```

- 命令提示符定义在环境变量 `PS1` 中
持久化命令提示符

```shell
\e # 控制符
\u # 当前用户
\h # 主机名简称
\H # 主机名
\w # 当前工作目录
\W # 当前工作目基名
\t # 24 小时时间格式
\T # 12 小时时间格式
!  # 命令历史数
# # 开机后命令历史数
```
在 centos 中写入 `/etc/profile.d/env.sh`
在 ubuntu 中写入 `.bashrc`

```shell
echo "PS1='\[\e[1;35m][\u@\h \W]\\$\[\e[0m\]'" >> .bashrc
```


# Shell
### 环境变量
```shell
$SHELL # 使用哪一种 Shell
$PS1 # 命令提示符
$PATH # 外部命令的搜索路径

```

内部命令: Shell 中包含的指令. 随着 shell 加载到内存已经加载在内存中
外部命令: 有独立的磁盘文件, 需要从磁盘中读取的命令, 理论上说速度慢
> 有的命令可能即是内部命令, 优势外部命令. 

命令的执行顺序： Shell 会先查看命令别名， 然后内存中寻找内部命令, 如果找不到, 最后就在环境变量 Path 中定义的目录中找

```shell
enable # 显示所有内部命令
type echo # 查看是否是内部命令, 如果别名, 则只显示别名
type echo -a # 查看内部和外部命令, 解决只显示别名的问题
echo is a shell builtin # echo 是内部命令

type who
who is /usr/bin/who

```

所有的使用过的命令都会被记录到 Shell 的缓存中
```shell
# 查看所有缓存在 shell 中的命令. hash 的是命令的路径,加快后面命令的定位
hash
```

> 如果发现命令执行不了了, 可能是命令的路径被修改了. 一种方法是清楚 hash 的缓存, 或者退出 shell

```shell
hash -r # 删除Shell 中所有缓存的命令
```

### 命令别名
```shell
# 临时增加别名
alias cdnet="cd /etc/sysconfig/network-scripts/"

# 列出所有的别名
alias 
alias <command> # 列出某个命令的别名

# 一般情况下bash中运行的命令是别名
# 强制运行原始命令， 不运行别名
\ls
'ls' # 和上面的效果一样
```
需要持久化的时候, 需要写入`.bashrc`
注意: 需要
### 退出命令
```shell
ctrl + d # 正常退出
ctrl + c # 强行退出
```
### 批量执行
```shell
cmd1;cmd2;cmd3
```
### 长命令换行
```shell
hostnamectl \
set-hostname \
newhost
```

### 命令帮助查询
- 内部命令使用help查询
- 外部命令使用man查询
- 使用`type -a` 区分是内部还是外部命令
```shell
# 使用whatis 之前需要构建man数据库
mandb 
whatis rm # 回复命令的作用简述

# 内部命令
help cmd
# 外部命令
cmd --help
```

# Storage 存储
### 硬盘分区
默认情况下, 硬盘的命名为 `sda`, `sdb`, `sdc`延续下去
一个硬盘下, 有多个分区. 记为 `sda1`, `sda2`, `sda3`

```shell
# 1. 查看所有硬盘信息, 包括插在设备上的 U盘. 这些硬盘对应文件 /dev/xvdb
lsblk 
# 2. 在硬盘上创建分区
fdisk /dev/xvdb
n # 创建新的分区
p # 主分区， 其他选默认
w # 保存
# 3. 更新分区表
partprobe
# 4. 在分区上创建文件系统
mkfs -t ext4 /dev/xvdb1
```
挂载分区
```shell
# 5. 创建目录用于挂载文件
mkdir /app

# 6. 查找UUID， 用于保存在 /etc/fstab中
blkid /dev/xvdb1
/dev/xvdb1: UUID="fa1e16c0-8c76-42c3-9816-bceda1a21fb8" TYPE="ext4"
# 7. 在/etc/fstab 中增加下面的内容
UUID=fa1e16c0-8c76-42c3-9816-bceda1a21fb8 /app  ext4    defaults.grpquota       1       2
# 8. 加载所有新的文件系统 -t = type
mount -t ext4 /dev/xvdb1 /app
# 9. 修改目录所属的组
chgrp app /app

```
### Quota
```shell
# 1. 安装quota
yum install -y quota
# 2. 创建quota文件
quotacheck -cug /app
# 3. 

```
### 识别新硬盘
添加硬盘后, 系统不会马上识别, 重启后可以识别
可以通过磁盘扫描的命令来实现
```shell
# 可能要执行好多次下面的命令， 使用不同的host值才能扫描出来
echo '- - -' > /sys/class/scsi_host/host0/scan
echo '- - -' > /sys/class/scsi_host/host1/scan
echo '- - -' > /sys/class/scsi_host/host2/scan
lsblk # 查看新的硬盘是否出现

# 定义别名来实现3个命令
alias scandisk="echo '- - -' > /sys/class/scsi_host/host0/scan;echo '- - -' > /sys/class/scsi_host/host1/scan;echo '- - -' > /sys/class/scsi_host/host2/scan"

```
## LVM 虚拟卷管理
### RAID
- 如果服务器做了 RAID, 那么所有的物理硬盘就会变成一个逻辑上的磁盘. 这个逻辑上的磁盘也就是虚拟卷LV
- RAID可以坐在软件层面和硬件层面. RAID的概念
- Mirroring 数据被完全的复制
- Striping 数据被放在多个磁盘中
- Parity 专用的校验数据和恢复数据, 可能使用独立的磁盘来存储

####  RAID0
-  No redundancy, 磁盘的利用率最高
- 读和写可以在不同的磁盘区块完成, 提供IO效率
![[Pasted image 20250210063805.png]]

#### RAID 1
- 完全备份和复制, 磁盘利用率低
- 读的效率提高, 因为可以任意从两个阵列中读取. 写的效率降低, 因为所有的修改都要在两个整列中写入
![[Pasted image 20250210064321.png]]
#### RAID2 & RAID3 & RAID4
- 用的很少
- RAID2 是Bit level parity check
- RAID3 是 Byte level parity check, dedicated parity disk
- RAID4 是Block level parity check, dedicated parity disk
![[Pasted image 20250210065459.png]]

#### RAID5
- 至少要3个硬盘. 最多可以允许一个硬盘坏了. 当时重构数据需要花很长的时间
- 在安防监控中经常使用RAID
- no dedicated parity disk
![[Pasted image 20250210070043.png]]

#### RAID6
- 类似于RAID5, 可以是有两个Parity Drive. 最多可以允许坏两个disk
- 读性能增加, 写性能减弱. 因为有两个parity drive要写入
![[Pasted image 20250210070205.png]]

#### RAID10
- 是RAID0和RAID1的结合. 至少需要4个盘. 2个盘存数据, 2个盘存mirroring

## LVM
- VG Virtual group 虚拟卷组, 这个组里面可以有多个虚拟卷. 每个卷如果磁盘的分区, 是相互独立的
- LV Logical volume 虚拟卷. 这个就是独立的分区了
# File Management
# 文件操作

```shell
ls -al # 查询当前目录下所有文件包括隐藏的

# 通配符
ls fil? # ? 表示单个字符
ls f* # 匹配任意长字符
ls file[1-9] # 匹配 file1, file2, file3, 只能匹配单个数字
ls *[[:digit:]] # 单个数字


ls | grep -E '^file' # 查询所有 file 开头的文件

```

```shell
# 创建1000 个文件
for i in {1..1000}; do touch file_$i; done

# 删除所有文件
rm file_[1-1000]
rm file_*
```

### 查看文件占用磁盘
```shell
# 查询指定目录下, 文件夹的磁盘占用
du -h --max-depth=1 <dir_path> | sort -rh | head -n 20
```

# 进程
```shell
# 显示所有进场
ps a 
```