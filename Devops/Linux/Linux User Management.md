
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
usermod -G <groupname> <username> # configure secondary group
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

remove user

```shell
# 删除和用户有关的进程
# 删除家目录
userdel -r <home_dir> <username>
# 查询所有和user有关的文件
find / -user <username>
```


其他工具
pwconv pwunconv
grpconv grpunconv


# visudo
直接修改 `/etc/sudoers`
把配置文件放入`/etc/sudoers.d`


# Centos 7 破解root账户密码

1. 重启设备 按e, 进入内核参数修改模式
![[Pasted image 20250131073810.png]]
1. 内核参数修改, 找到linux16 所在的段, 把`ro` 改成`rw`, 在后面加上 `init=/bin/sh`. 用户ctrl + x 进入单用户模式
![[Pasted image 20250131073822.png]]
3. 使用 `passwd` 修改root账户的新密码
4. `touch /.autorelabel` 创建表现文件, 是selinux允许对root密码的修改
5. `exec /sbin/init` 重启