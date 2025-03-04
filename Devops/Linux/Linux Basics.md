# GUI
```shell
ctrl + alt + f3 # 切换到非 gui
ctrl + alt + f2 # 切换到gui
```

# Linux SSH

### Install openssh
在服务器侧安装服务端
```shell
sudo -i
yum -y install openssh-server openssh-clients
systemctl start sshd
systemctl status sshd
```

远程登录服务器
注意: Ubunut 不允许root账户远程登录, 需要用别的账户
```shell
ssh username@<ip>
```

使用公钥文件登录ssh
```bash
chown 400 perm # 400 means read and execute
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
- Linux的用户分User和Group
- Linux给每一个用户都会分配一个ID， 也成为UID. 系统是根据 UID 来判断各种权限的, 而不是根据名字
- 如果一个新的用户, 想继承一个老用户的所有权限, 只要改名, 不改 UID 就行

UID的范围 
- 0 是superuser root用户
- 1-200 是系统进程
- 201-999 没有任何文件的系统进程
- 1000+ 普通用户
注: 一般在一个服务器上跑一个服务, 担心不同的服务互相影响, 影响性能
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

3A
- Authentication 认证, 验证用户的身份
- Authorization 授权
- Accounting| Audition 审计
token
- 当用户验证身份之后, 服务器发送这个用户token
- 用户用这个 token 去获取资源

#### SU 切换用户

```shell
# 不完全切换, 保留原来的路径, 保留原来 bash, 保留环境变量
su <username>
# 完全切换
su - <username>
# 切换一个命令, 然后退回来, 不需要来回切换
su - <username> -c 'touch /tmp/a.txt'
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
查找用户或者组
```shel
getent passwd mysql # 查询用户 mysql 的信息
```
组成员管理
```shell
groupmems -l -g mail # 列出把 mail 作为附加组的组成员
# 给组添加成员
groupmems -a <username> -g <group_name> 
groupmems -d <username> -g <group_name>
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
chown -R userb:userb dir # 修改目录及目录里的所有文件和文件夹的权限

chmod 700 /userb # 给userb所有的权限

# 锁定账号
usermod -L <username>
getent shadow <username> # 密码前会有!
usermod -U <username> # 解锁账号

# 修改用户名, 但是需要手动改家目录, 但是权限一样
usermod -l current_name new_name

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
# change the primary group for user
uermod -g <username> <groupname>
# 只有当没有用户使用该组为主组是, 才能删除这个组
# remove group
groupdel <groupname>
```

对于一些服务, 创建用户和组的实例
```shell
# -r 表示系统组
/usr/sbin/groupadd -g 82 -r postfix 2>/dev/null # 指定了新建组的编号
# -M 不创建家目录中的那些普通用户所需要的文件, 但是-d 还是指定了家目录的地址
# -r 系统账号, 不会创建普通用户那样的文件, 比如邮箱
/usr/sib/useradd -d /var/spool/postfix -s /sbin/nologin -g postfix -G mail -M -r -u 89 postfix 2>/dev/null
```

#### 家目录中的文件
- `.bash_logout` 退出shell时会执行的命令
- `.bash_profile` 第一次登录shell的时候, 会执行的命令和变量. 该用户新建一个shell的时候, 不会被执行
- `.bashrc` 用户每次新建一个shell的时候, 都会被执行
在 `/etc/skel`下定义了每个新用户的创建在家目录中会创建默认文件的模板
- 如果要让新建的家目录中含有某个默认的文件, 就要在 `/etc/skel`下创建一个文件
- 系统在创建新用户时的行为定义在 `/etc/default/useradd`
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
# 3. 删除用户和家目录和邮箱
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
# 一般不在 useradd 中使用 -p 来设置秘密, 因为这里需要填写的是加密后的密码
# 使用 passwd 来设备密码, 输入的密码会被加密, 放到 shadow 文件中
passwd userA # 设置新密码
# 使用管道符来一次性创建密码
echo <password> | passwd --stdin <username> # ubuntu不支持, rocky 支持
echo -e '123456\n123456' | passwd <username> # 通用, ubuntu 和 rocky 都支持
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
- 新建一个文件默认是不加执行权限的, 因为文件的执行权限有安全风险
- 新建文件的默认权限是 644, 新建文件夹的默认权限是 755
- 一个文件能不能删除, 是由目录决定的
-  文件夹读写执行的含义
	- 读 - 列出文件列表. 但是看不到 inode, 和文件的 meta 信息
	- 写 -  在目录下建立新的文件, 或者删除文件
	- 执行 - 看不到文件夹列表, 但是可以看到里面的文件的内容和属性, 如果你知道文件名
	- 所以一般要能访问目录, 一般会给 rx
- 如果只相对文件夹内的所有子文件夹加 x 权限, 对文件不加 x 权限, 可以使用 X 选项
- 
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
# 把 u 的权限都去掉, g 增加 w 权限, o 给所有权限
chmod u=,g+w,o=rwx <filename>
# 制定用户和群各自不同的配置
# 分别置顶用户的权限是7，群的权限是5， 其他用户的权限是0
chmod 750 <filename> 

# 修改目录的权限
chmod -R 777 <dir_name>

chmod -R a+X dir/

# 增加 s 权限
chmod u+s /bin/cat # 让所有人都有 root 权限查看文件
chmod u-s /bin/cat # 删除权限
# 如果用数字表示
chmod 4755 /bin/cat
chmod 755 /bon/cat # 删除 suid 权限
```
umask 用来修改新建文件和文件夹的默认权限
```shell
umask
0022 # 第一个的 0 表示 8 进制
# 文件的默认权限是 666-umask, 如果检出的结果中有奇数,所有奇数位+1, 加 1 的目的是去掉执行权限
022+644 = 666 # 666 本身就把
# 文件夹的默认权限是 777-umask
022+755 = 777 
```
suid 权限
```shell
ll `which passwd`
# 这里有个 s 权限, 表示当用户执行这个命令时, 会临时使用文件所有者的权限, 这里是 root
-rwsr-xr-x 1 root root 68208 Feb  6  2024 /usr/bin/passwd*
```
### sgid 权限
- 创建新文件的所属组是用户的主组
- sgid 作用在文件和文件夹的行为是不同的
	- sgid 作用在文件时, 文件执行时, 临时继承文件所属组的权限
	- sgid 作用在文件夹时, 在文件夹内创建的文件, 自动继承文件夹所属组的权限
```shell
chmod g+s # 继承组的权限
chmod 2755 # 2 表示 sgid
chmod 6755 # 又有 suid 和 sgid
```
### sticky 位
```shell
ll -d /tmp
# 这里的 t 是 sticky 位, 表示这个目录里的文件, 只能删除自己的, 不能删除别人的
drwxrwxrwt 14 root root 4096 Feb 28 09:56 /tmp/

chmod 1777 /tmp # 增加 sticky 位
chmod 777 /tmp # 去除 sticky 位
chmod o+5 /tmp # 模式法 增加 sticky 位
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

#### 特殊权限
设置文件的特殊属性, 可以防止 root 用户误操作删除或者修改
```shell
# 不能删除, 改名, 更改
chattr +i file
chattr -i file # 删除这个属性, 就可以删除 
lsattr # 会出现 i 的标记
# 能追加, 但是不能删除
chattr +a file


```
### File ACL
- 实现只显示某个用户的权限
- 当给文件设置ACL之后，`ll` 会出现 `+` 标识

```shell
# 查看文件是否设置 acl 权限
getfacl <filename>

# m for modify
setfacl -m mask:rw <filename>
# 删除 testuer1 对这个文件的权限
setfacl -m u:testuser1:- <filename>
#  给 testuser1 配置 rw 权限
setfacl -m u:testuser1:rw <filename>
# 删除 mage 在 acl 控制
setfacl -x u:mage <filename> 
# 删除所有在文件testfile1上的关于group test的ACL
setfacl -x g:test testfile1

# 清除文件所有的 ACL 权限
setfacl -b <filename>
```

 案例
```shell
# 如果删除了 chmod 的可执行权限
chmod -x "which chmod"
# chmod +x 也就不能使用了
# 此时就要用 acl把权限加回来
setfacl -m u:root:rwx /usr/bin/chmod
chmod +x /usr/bin/chmod
```

#### 文件属性中的selinux标签
- selinux 在安装完成后, 默认是启用的, 在生产中一般是禁用的
```shell
# 查看 Selinux看是否 Diable
getenforce

# 在 selinux 被禁用后, 创建的新标签, 没有这个.
# 属性中最后又一个.是 selinux 的标签,用下面的命令可以看到
ll -Z ananconda-ks.cfg
```
禁用 Selinux
```shell
cat /etc/selinux/config
# 把里面的
SELINUX=enforing
# 改为
SELINUX=disbled
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
- man根据不同的命令类型, 分了9个大类, 默认如果不指定类别的话, 返回类别1
- man帮助的路径是由man帮助的配置文件 `/etc/man_db.conf` 中定义了搜索man帮助文件的路径
```shell
# 使用whatis 之前需要构建Whatis 数据库
mandb 
# 回复命令的作用简述, 帮助文档的类别, 可以用man使用
whatis rm 
# 查看命令帮助文档路径
where is rm
# 内部命令
help cmd
# 外部命令
cmd --help
# 外部命令
man <cmd>
# 对于手工安装的软件比如, Nginx, 可以直接制定帮助内容路径
man <path>
```

### 时间和时区
- 时间也区分硬件时间和软件时间

```shell
date # 查看日期
date +%Y-%m-%d # [+Format]
data +%F # 和上面一样
# 查看硬件时间 Bios中的时间
clock
# 用硬件时间去矫正软件时间
clock -s
# 用软件时间来矫正硬件时间
clock -w
# 查看时间
timedatectl
# 配置时区为马德里
timedatectl set-timezone Europe/Madrid
cat /etc/timezone
ls -l /etc/localtime # 查看的软连接

# 显示日历
cal
# 显示 2023 年日历
cal 2025
```

### 修改motd信息
```shell

# issue的信息是在登录之前就显示的
cat /etc/issue 
# motd的信息是在登录时显示的
cat /etc/motd 

```
### hostname
```shell
# 显示主机名
hostname 
# 修改主机名
hostnamectl set-hostname <hostname>

```
长时间执行的命令, 关闭窗口还能执行
```shell
yum -y install epel-release.noarch
yum -y install screen
screen

ps aux | grep ping # 查看程序是否在运行
```
建议学习 Tmux

### 查看硬件信息
```shell
# 产看cpu信息
lscpu 
# 等同于 
cat /proc/cpuinfo

# 产看内存
free -h
cat /proc/meminfo
# 关闭图形界面
init 3
# 打开图形界面
init 5

# 查看分区信息
df -h
cat /proc/partitions

# 查询内核版本
uname -r
# 查询架构
uname -p
# 查询发型版本
cat /etc/os-release
lsb_release -a

# 重启
reboot
init 6
shutdown -r now # 立即重启
# ctrl + alt + delete 在生产中物理机可以重启

# 关机
halt
poweroff
init 0
shutdown # 一分钟以后关机
shutdown -h now # 立即关机
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
ps aux
```

echo
```shell
echo $PATH
echo "$PATH" # 可以识别出变量
ehco '$PATH' # 只打印字符串

touch `date +%F`.log # 反向单引号为另一条命令的执行结果
touch $(date +%F).log # $() 和反向单引号是等价的, 但是建议使用, 因为可以嵌套
```
计算器
```shell
echo 7*12 | bc # bc 是计算器
```

编码
unicode中包含 utf-8, utf-16, utf-32 其中 utf8是1到 4 个字节.包含了了所有国家的字符集. 
- GB3212只是中国的文字的字符集
- IOS8859-1 欧洲的字符集
```shell
echo $LANG
```
- windows 中, 如果按回车, 会增加 `\r\n`, linux只是`\n`, 所以在 windows 中编辑的文本文件放在 windows 中, 容易出问题
```shell
hexdump -C text.txt # 可以查看文件 16 进制表示
```

扩展符号
```shell
echo {1..10} # 10个数字
echo {10..1..2} # 10 8 6 4 2
echo {a..c}{1..3}
touch a{1..10}.log # 创建10 个文件
```
 history
```shell
!s # 执行最近一次 s 开头的命令
!?con # 执行最近一次命令中包含 con 字符的命令
echo !^ # 前面一个命令的第一个参数
echo !$ # 前面一个命令的最后一个参数
echo !* # 前一个命令的全部参数

# 快速获取前一条命令的参数 esc (松手)+ .
cat /etc/motd
ll /etc/motd
```

bash 快捷键
```shell
ctrl + l # 清屏
ctrl + a # 光标移动到命令行首
ctrl + e # 光标移动到命令行末
ctrl + > # 光标向右移动一个单词
ctrl + < # 光标向左移动一个单词
ctrl + u # 光标删除直命令行首
ctrl + k # 光标删除直命令行末
ctrl + d # 向后删除

ctrl + s # 锁死屏幕, 屏幕不会显示
ctrl + q # 两次 解锁屏幕
```


# Linux 文件系统
- `/bin`和 `/sbin` 的区别是, `/bin` 是普通用户可以还行, `/sbin` 是管理员才能执行的程序
- linux 内核文件保存在 `/boot`下, 只有 12M
```shell
/bin # 存储 cd, mv 等命令的的二进制文件, 比如 ls 命令, 就是/bin/ls 文件
/sbin # 系统管理员使用的命令的二进制文件
/usr  # used for sharing files between users
/media # usb 介质
/home # 所有用户的文件夹都放在 home 下面
/etc # 相当于 windows 中的注册表, 存储 linux 所有程序的配置文件
/boot # 存放开机启动需要的程序, linux 内核也在里面
/run # 程序运行的时候的临时文件
/tmp # 用户和程序的临时文件
/var # 日志等文件
/proc # 吧保存在内存中的文件, 用于进程
/sys # 保存在内存中的文件, 用于硬件
```

`/dev`下有块设备, 字符设备
块设备, 比如硬盘. 一个块包含多个字节
字符设备, 一个字符一个字符为单位 比如 `/dev/zero`, /dev/pst/1

蓝色: 文件夹, 绿色: 可执行程序, 红色: 压缩文件, 浅蓝色:链接 `/etc/DIR_COLORS` 定义

```shell
\ls # 原始的 ls 命令
ls # 其实是别名 ls --color=auto
```
### File Privilege
Linux 文件权限有三个维度, 使用 `chmod`修改
- user是文件的所有者
- group 是组
- other 为其他

```shell
-rw-rw-r-- # 三个组 User Group Other
```

修改文件权限
```shell
chmod u+x filename.txt # 给 user 添加执行权限
chmod go+x filename.txt # 给 group 和 other 增加执行权限
chmod ugo-wr <filename> # 删除 user, group, other 的 write, read 权限
chmod +x <filename> # 给文件所有的组加上执行权限

# 修改文件的组
chgrp groupname filename
chown .groupname filename
```



Linux 文件的所有权有两个维度. 使用`chown`修改
- user
- 组

文件类别
```shell
- # 普通文件
s # socket, 双向实现两个程序通信
c # 字符设备文件, 比如/dev/pst/0, /dev/null, /dev/zero
b # 块设备文件
p # 管道文件, 单向的. A程序把数据输入管道文件, B 从这个管道文件读出数据
l # 链接文件
d # 文件夹
```

文件路径
```shell
basename <path> # 文件basename
dirname <path> # 文件夹的父目录


cd ~username # 进入某个用户的家目录
cd # 进入当前用户的家目录
cd - # 回到上次的路径 

ls -R <path> # 列出嵌套的目录

```

文件的属性称为文件的 metadata, 元数据
文件的三个时间 
- 内容修改时间 mtime
- 读时间 atime, 更新条件: 读时间超过一天以上, 或者修改时间新于读时间
- 属性修改时间 ctime - 比如修改所有者

```shell
# 显示文件的读时间
ll --time=atime filename
# 显示文件的属性修改时间
ll --time=ctime filename
# 显示文件详细的信息
stat /etc/networks 
  File: /etc/network
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: fd00h/64768d    Inode: 131148      Links: 4
Access: (0755/drwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2025-01-21 01:19:22.637164053 +0100
Modify: 2023-03-14 23:42:54.000000000 +0100
Change: 2025-01-21 01:19:22.637164053 +0100
 Birth: -

```
文件的节点编号 inode
- 可以认为是文件的唯一标识符. 默认是不显示的
- 节点编号和分区的大小有关
- 如果有一个块硬盘是 sda, 则分区为 sda1, sda2
- 可能存在硬盘没有放满, 但是节点编号被用完了. 报错 `no space left on devicek`
- 删除文件后, 节点编号会释放. 但是硬盘空间不一定会被释放. 如果有程序还在使用这个文件
- 可以通过查看根目录的本地目录和上级目录判断根目录的上级目录就是自己

```shell
# 显示文件的节点编号 inode
ls -i 
# 每个分区可用的节点编号的最大值, 节点编号用量
df -i
# 查看每个分区的空间占用
df -h
# 查看文件类型,如果是是设备文件就不能用 cat 查看
file <filename>
```
文件通配符
```shell
* # 0 或者任意字符, 但是不包好, .开头的隐藏文件
? # 单个字符
[0-6] # 0到 6 中的一个数字
[124] # 1或者 2, 或者 4
[^12] # 不是 1,不是 2
[c-f] # c到f, 包含大写字母. 先出现小写字母后出现大写字母 cCdDeEf
[:lower:] # 所有小写字母你[[:lower:]]表示所有小写字母中取一个
[:upper:] # 左右大写字母 [[:upper:]]表示所有大写字母中取一个
[:digit:] # 数字


.* # 注意 表示包含所有.开头的隐藏文件, 且包含父目录.., 如果此时是在家目录下做操作, 则可能删除根目录

```

复制
```shell
# 复制文件
cp -a file file.bak # 保留所有属性备份
cp -a file{,.bak}

# 复制目录
cp -r path/ new_path/ # 如果目录重叠, 则会放在重叠目录下的子目录
# 先备份再覆盖
cp -b file1 file2 

```
特殊文件的复制
```shell
ll /data/zero /h
# 对于特殊文件的复制, 要保留原属性
cp -a /dev/zero /data/

```
移动
- 可以有多个源, 但是目标只有一个
多个文件改名, 改成不同的
```shell
# centos
rename .txt .txt.bak *.txt # 多有*.txt 文件中的.txt 替换为.txt.bak
rename .bak '' *.txt.bak # 把所有的.txt.bak 文件改成 .txt
# ubuntu 上需要额外安装
rename 's/\.txt/\.txt\.bak/' *.txt
```

删除
一般在公司里, 不会先删除. 而是先关机, 移动到空闲的磁盘空间, 1 个月. 然后再删除. 

```shell
rm ./-a # 删除看上去像 option 的文件

# 用 mv 来取代 rm 防止误操作
alias rm='mv -t /tmp' # mv -t 作用是先写 destination 目录, 后写源目录
```
删除大的临时的文件的正确方法
- 规避删除文件后, 还有其他程序占用文件, 造成无法释放硬盘空间
```shell
# 删除之前事先清空文件
cat /dev/null > <detination_file> 
rm -f <destination_file>

# 删除文件后, 手动释放文件占用
lsof | grep delete # 可以看到进程编号
# 再用 ps 命令找到指定进程, kill 掉该进程
# 但是这种方法, 可能 kill 的进程是数据库服务, 不能轻易停
```
创建文件夹
```shell
mkdir -p /a/b/c
```
标准 IO 重定向
- 有标准输入, 标准输出, 标准错误都通过文件来标识
- 三种输出对应 0, 1, 2, 也称为文件描述符. 文件描述符是一个进程为每一个打开的文件分配的唯一标识(数字), 前三个数字是固定的
- 默认情况下, stdin, stdout, stderr都是默认输出到当前窗口的. 重定向就是要不输出到窗口, 重定向给指定的程序
```shell
/dev/stdin # 标准输入设备文件(键盘输入), 比如 bc, cat
/dev/stdout # 标准输出设备文件
/dev/stderr # 标准错误设备文件

# 标准输入对应 0, 标准输出对应 1, 标准错误对应 2
ls /dev/std*
lrwxrwxrwx. 1 root root 15 Feb 21 07:17 /dev/stderr -> /proc/self/fd/2
lrwxrwxrwx. 1 root root 15 Feb 21 07:17 /dev/stdin -> /proc/self/fd/0
lrwxrwxrwx. 1 root root 15 Feb 21 07:17 /dev/stdout -> /proc/self/fd/1


# 找到程序对应的进程编号
pidof tail
ls /proc/1759/fd # 查看进场编号为 1759 的文件描述符

# 把标准输出重定向到别的 tty, /dev/pts/1 是一个设备文件
hostname 1> /dev/pts/1 
hostname > /dev/pts/1 # 标准输出 1 是可以省略的

# 把标准错误信息重定向到 err.txt, 注意标准输出不会被重定向
ls /dataa 2>err.txt 
# 如果一个命令中有多的有错的, 正确的输出到一个文件, 错误的放在另一个文件里
ls /data /dataa
ls /data /dataa >stdout.txt 2>stderr.txt
ls /data dataa &>all.txt # 标准输出和错误输出都在同一个文件里
# 老的写法, 标准输出和标准错误都写在同一个文件里, 注意前后次序不能变
ls /data dataa >all.txt 2>&1 #


# 标准输入的重定向
echo 2+3>bc.txt
bc <bc.txt # < 是标准输入的重定向, 把文件中的内容输入给 bc
# 从 1+2+...+100
seq -s+ 100 >bc.txt;bc<bc.txt
# 或者使用重定向高级用法
bc <<<`seq -s+ 100`
# 右边公式的结果临时保存在一个文件内, 作为标准输入函数的输入
bc < <(seq -s+100)
```

shell
```shell
# 安装 cshell
yum -y install csh
# 查看当前shell
echo $SHELL 
ls /etc/shell
csh # 切换为 cshell

>a.txt # cshell 不支持这种清空文件的写法
```

多行重定向
- `>`是单向重定向, 每次按回车, 内容就会输出到文件
- `<<EOF`其中 EOF 表示重定向结束, 是一个习惯
- EOF后面不能包含空格 
```shell
# 覆盖
cat > a.txt <<EOF
newline1
newline2
newlo3
EOF # 当重启一行输入 EOF 时, 才会把内容输出到文件

# 追加内容
cat >> a.txt <<EOF
newline1
newline2
newlo3
EOF # 当重启一行输入 EOF 时, 才会把内容输出到文件
```

管道符
- 把命令 1 的输出结果作为第二个命令的输入. 也就是管道符后面的命令一定是支持标准输入的
- 管道符左面的命令一定是标准输出功能的
```shell
echo 1+2 | bc
cat mail.txt | mail -s subject receiver@email.com
```
其他标准输入命令
```shell
# tr 命令用来转换
tr a-z A-Z < filename # 小写转换为大写
tr -d abc < filename # 删除 abc
tr -dc abc < filename # 除了 abc, 其他都删除
tr -s abc < filename # 发现连续的 a, 连续的 b, 连续的 c, 压缩成一个
tr -s ' ' <filename # 压缩空格
df | tail -n +2 | tr -s ' ' | cut -d" " -f5 | tr -d %

# tee 命令 支撑标准输入
tee out.txt
hello # 同时在屏幕上打印和输出到文件
# 追加内容
echo hello3 | tee -a new.out | tr 'a-z' 'A-Z'

# 使用 tee 命令生成配置文件
tee /data/text.conf <<EOF
line1
line2
EOF

```


### 软链接和硬链接
硬链接
- 把一个文件起另外一个新的名称, 节点编号是相同的, 文件是同一个. 两个硬链接是平等关系
- 删除其中一个, 并不会删除文件. 除非把所有的名字都删除了. 节点编号则会被回收, 文件才会被删除
- 删除一个文件后, 文件的数据并没有立即被清空. 利用专业的数据恢复软件, 文件可以被找回
- 创建硬链接之后, 文件的连接数会变化. 如果应该文件有多个硬链接, 连接数会大于 1
- 硬链接但是必须在同一个分区中. 不能跨分区不能创建硬链接
- 硬链接只支持文件, 不支持文件夹
```shell
ln target hardlink # 创建硬链接文件
```
软链接
- 软链接也称为符号链接. 类似 windows 的快捷方式
- 在升级版本时, 可以把原来的软链接删除, 为 app 指向新的文件夹, 创建一个软连接. 
- 删除软连接的方法, 只能删除软链接文件`rm -f app`, 不是删除软连接下的文件 `rm -rf app/`
- 创建软连接要么使用绝对路径, 或者相对于目标软链接的相对路径
- 删除源文件, 软连接失效
- 软连接可以跨设备, 跨分区

```shell
ln -s target softlink 
# 给老的版本创建一个软连接
ln -s app1.0 app
# 删除软链接,版本文件夹保存
rm -f app
# 为新的版本文件夹创建一个软连接
ln -s app2.0 app

# 回退到老的版本
rm -f app
ln -s app1.0 app
```


# 邮件
- 邮箱保存在 `/var/spool/mail`, 每个用户都有一个文件夹. 收到的邮件都在这些文件夹下
IMAP/SMTP 需要授权码
```shell
# 安装
yum -y install postfix;systemctl enable --now postfix
```
配置邮箱
```shell
# rocky 
# 查询某个配置文件属于哪个软件
rpm -qf /etc/mail.rc
# 会提示出这个文件是 mailx-12.5-29.el9.x86_64这个软件装的
# 安装一下这个软件后, 才能编辑这个配置文件
vim /etc/mail.rc
# 贴入下面的配置
set from=29355@qq.com
set smtp=smtp.qq.com
set smtp-auth-user@29355@qq.com
set smtp-auth-password=dfasdfasdf
```
# 文件搜索
### Locate
- 搜索速度非常快, 需要首先构建文件索引数据库
- 搜索依赖于数据库
```shell
updatedb # 构建或者更新数据库
locate <filename> # 搜索文件
locate -r '\.conf$' # 支持基本正则表达式
```

### find
- 优势在于搜索条件丰富
- 直接搜索磁盘
- 默认就是递归搜索当前目录下的所有文件
- 
```shell
find # 
find /etc/ -maxdeptch 1 # 只搜索目录本身, 算作第一级目录
find /etc/ -mindeptch 2 # 只搜索第二级

# 根据文件名查找
# 默认情况下是匹配整个路径, 比如 /etc/chrony.conf
find /etc/ -regex ".*\.conf$" 
# 精确匹配文件名, 不用匹配路径
find /etc/ -name passwd 
find /etc/ -name "*.conf" # 支持正则, 必须有双引号
find /etc/ -type f -iname "example.txt"

# 文件的所有者, 所属组
find /var/ -user wang -ls 
find /home -nouser -ls # 搜索没有用户的文件, 用户被删除后遗留下来的

# 搜索文件类型
find /etc/ -type d -ls # 只是文件夹
find ~ -type f -ls # 只是文件, 不包含文件夹

# 空文件
find -empty -ls 

# 多个条件
find /home \(-user wang -o -name "*.conf"\) -ls # -o 表示或者
# 查找空文件夹内
find !\(-type d -a -empty \) # -a 表示 and
# 跳过 security 文件夹来搜索其他目录, -prune 去掉这个目录
find /etc -path '/etc/security' -prune -o -name "*.conf"


dd if=/dev/zero of=f1.img bs=1k count=1024 # bs表示块的大小, count 表示有 1024 个块, 等于 1M 的文件
find -size 2M # 大于 1M 到 2M 之间的文件
find -size 2048K # 2047K 到 2048K 之间的文件
find -size +6k # (6k, 无穷大)
find -size -6k # (5K, 6K)

# 按照时间搜索
find -atime +30 # 30 天前触碰过的文件
find -mtime -7 # 过去7天内修改过的文件 

# 按照权限搜索
find -perm 444 # 根据权限来搜索
find -perm /444 -ls # 或的关系. 只要user, group, other 的权限大于 4 的就行
find -perm -444 -ls # 并且关系
find -perm /600 -ls # user有 r 或者 w 的权限, 0 表示任何权限都可以
find -perm -600 -ls # user表示必须有 r 和 w 的权限
find /usr/bin -type -f -perm /111 # 搜索所有可执行文件

# 和其他操作一起实现
find /home -type f -name "*.sh" -exec chmod +x {} \; # 给所有.sh文件增加可执行操作权限, 这个 \;一定要加, 表示 -exec 的命令结束了 

find -size +100M -size -130M -ls # 大于 100M 小于 130M 的文件

# 列出目录下所有文件, 并按照文件大小排序
find /etc/ -type f | xargs ls -Sl 

```

### xargs
- 因为不是所有的命令都支持标准输入. 用 xargs 结合标准输入重定向, 可以把一条命令变成支持标准输入的命令
- touch, ls 这些命令虽然可以跟多个参数, 但是参数是有极限的. 比如不能是几十万个, 这时可以用 xargs 每次为命令喂1 万个参数

```shell
# ls 命令本身是不支持标准输入的
xargs ls -l # 转变为标准输入
echo a.txt | xargs ls -l # 把 a.txt 的内容变成 xargs 的标准输入

# -n3 等于每次输入一个参数
seq 10 | xargs -n3 # 每行 3 个

# 一次创建 10 个用户
echo user{1..10} | xargs -n1 useradd # -n1 表示每次输入 1 个参数, 调用 10 次
```
一个 python 下载视频工具的案例
```shell
# -i 和 {} 连用, 表示替换
# -P3 同时调用 3 个进程
yum install -y python3-pip
pip3 install you-get
seq 60 | xargs -i -P3 you-get https://www.bilibili.com/video/BV14K11w7uF?p={}
```
使用 null 或者 ascii 码中的 0 作为分隔符, 用于规避文件名中的空格, 影响了分隔符
```shell
# -0 xargs 指定 \0 为分隔符
find -type f print0 | xargs -0 ls -l
```
# 压缩
- compress 压缩成.z 文件, 效率不高
- gzip, 互联网上常用, 支持压缩比, 但是压缩比高的时候, cpu 代价高. 可以接受标准输入
- bzip, 也支持压缩比, bz2 文件, 非标配, 需要额外安装
- xz 压缩比更高
- zcat 压缩文件预览
- zip 可以压文件, 也可以压文件夹
```shell
# compress
yum install -y compress
compress <filename> # 生成的文件.z, 源文件丢失
compress -c <filename> > <zfile_name> # -c 是在屏幕上打印
uncompress <filename> 


# gz
gzip <filename>
gzip -k -9 <filename> # centos8 之后, 保留原来的文件. -9 表示压缩比
gzip -d <filename> # 解压缩
# 可以对标准输入进行压缩
ls -R / | gzip > test.gz

# bzip
bzip2 -9 -k <filename>
bzip2 -d <filename>

# xz
xz -9 -k <filename>
unxz <filename>

# zip 
du -sh /etc/ # 查看目录大小
zip -r  zip_file.zip <dirname>
unzip zip_file.zip
unzip zipfile.zip -d <target_folder>
zip -p # 增加密码

```
# 打包
- c 打包
- t 预览
- f 文件
- v 看过程
```shell
# 打包
tar -cvf file.tar <dirname>
tar -tvf file.tar # 预览tar 文件中的内容
tar -xvf file.tar # 解包
tar -xvf file.tar -C <target_dir> # 指定解包的目录

# 打包目录下的文件, 不包含目录
tar -C /etc/ -cvf /opt/etc.2.tar ./ # 这里-C 表示进入 /etc目录 ./表示所有文件
# 调用压缩程序
tar zcf file.tar.gz /etc # 把目录打包, 压缩成 gz 文件
tar jcf file.tar.bz2 /etc # 调用 bzip2 来压缩, 需要先安装 bzip
tar Jcf file.tar.xz /etc# 调用 xz 来压缩, 需要先安装 xz

tar xf file.tar.xz -C <target_dir> # 通用解压 gz, bzip, xz 文件

```
拆分
```shell
split -b 1M -d filename filename_suffix # -d 表示数字小编号, 每个文件切成 1M
cat filename* > filename # 合并这些文件
```
# RPM
```shell
# 查看软件信息
RPM qi xz 
```
# 修改网卡名
```shell
vim /etc/default/grub 
# 增加 if.names=0
GRUB_CMDLINE_LINUX="crashkernel=auto resume=/dev/mapper/rl-swap rd.lvm.lv=rl/root rd.lvm.lv=rl/swap rhgb quiet net.ifnames=0"
# 需要执行后重启
grub2-mkconfig -o /etc/grub2.cfg; reboot
```
# 初始化

### Rocky
1. 最小化安装
2. 关闭防火墙
```shell
systemctl diable --now firewalld
```
3. 关闭 SELinux
```shell
nano /etc/selinux/config
SELINUX=disabled
reboot
```
4. 实现邮件通信
```shell
yum -y install postfix mailx
systemctl enable --now postfix
```
5. yum 源
```shell
CentOS8: BaseOS, appstream, epel
CentOS7: BaseOs, epel
```
6. 常用软件
```shell
yum -y install bash-completion psmisc lzsz tree man-pages redhat-lsb-core zip unzip bzip2 wget tcpdump ftp rsync wim lsof
```
7. 网卡 NAT
```shell

```
1. 时间同步

