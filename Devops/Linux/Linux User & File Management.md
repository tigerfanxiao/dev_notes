
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


