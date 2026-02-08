在mac上安装虚拟环境

```shell
brew install --cask multipass
multipass launch --name linuxlab
multipass launch \
  --name linuxlab \
  --cpus 2 \
  --memory 4G \
  --disk 20G

multipass list
multipass shell <vm>
multipass delete <vm>
multipass purge
multipass start <vm>
multipass stop <vm>
```

补全

```shell
apt install bash-completion -y # 补全功能
```

# GUI

```shell
init 3
init 5
```

# SSH

ssh 通信原理

1. 客户在向服务器发送ssh请求时, 在本地 `.ssh/`目录下, 生成 known_hosts 文件, 记录服务器的三套公钥和服务器的ip地址

### Install `openssh`

客户端

```shell
ssh user@ip -p 2202 # 定制端口

```

在服务器侧安装服务端

```shell
sudo -i
# for rocky 8
yum -y install openssh-server openssh-client
# for ubuntu or debian
apt-get install openssh-server openssh-client

systemctl start sshd # start ssh service
systemctl status sshd # show ssh service status

# 服务段配置文件
/etc/ssh/sshd_config
# 配置文件中的属性
PermitRootLogin yes # 允许root用户访问
AllowUser xxx # 如果配置, 表示只允许xxx用户访问
# 服务器公钥和私钥存放, 一般有3套工公钥
/etc/ssh/
```

文件传输

```shell
# 传输到远程主机上, 如果不写目录, 默认传输到家目录
scp local_file user@ip:/tmp

# 拉取远程主机上的文件
scp user@ip:/tmp/file
scp user@ip: # 默认传输到家目录, 注意冒号千万不能省略, 否则变成本地文件重命名

# 目录传输
scp -r /etc user@ip:/tmp
# 拉取远程主机上的目录
scp -r user@ip:/tmp
```

因为scp 不校验文件覆盖, 可以使用rsync工具来替代

```shell
yum install rsync
rsync -av host root@10.0.0.13:/data/ninhao # 会创建一个文件, 并保留原文件的属性
rsync -av /etc # 把目录一起传过去
rsync -av /etc/* # 传 /etc 下面的所有文件
```

使用密码远程登录服务器 - 验证passwd中的密码

1. 客户端向服务器发送连接请求, 输入密码后, 确认yes, 保存服务端发来的三套公钥, 保存在`~/.ssh/knownhosts`
2. 服务端会创建一个文件 `~/.ssh/authorized_keys` 但是文件中没有内容
3. 客户端再次登录的时候还是需要输入密码, 只是不用点yes
4. 使用密码登录服务器注意: Ubuntu 不允许root账户远程登录, 需要用别的账户

```shell
ssh username@<ip>
```

免密登录ssh - 使用证书

1. 在客户端本地生成私钥和公钥密码对 `ssh-keygen`
2. 将使用公钥文件发送给服务端 `ssh-copy-id -i ~/.ssh/id_rsa.pub user@ip`
3. 服务端把客户端发来的3套公钥保存在 `~/.ssh/authorized_keys中`
4. 客户段第一次登录时, 需要输入密码, 还是会有个yes, 把服务端发来的公钥保存在 `~/.ssh/knownhosts`
5. 客户端再一次登录时, 不需要再输入密码

```bash
ssh-keygen #

chown 400 perm # 400 means read
ssh usernanme@ip_addriess -i perm
```

复制公钥密码给远程用户, 实现跨主机免密登录

```shell
ssh-keygen # 在本地生成公钥和私钥
ssh-keygen -t rsa # 指定秘钥类型
ssh-copy-id root@10.0.0.12 # 将本机的公钥发给 ssh 服务端 ~/.ssh/authorized_keys中

ssh root@10.0.0.12 command # 测试远程连接, 发送命令

# add cloud_user identity to the agent and to reload the agent
eval $(ssh-agent -s)
```

海量设备互联

1. 如果3台设备之间互联, 每台设备需要维护自己的一套私钥和公钥, 剩余2台主机的公钥, 一共2+2个秘钥. 所有设备12个秘钥
2. 如果是n台设备之间互联, 则每台设备需要维护自己一套私钥和公钥, 剩余n-1台主机的公钥, 即本地n+1个公钥, 所有设备 n^2 + n 个秘钥
3. 为了解决大量的秘钥维护问题, 我们把所有节点使用同一套公钥和私钥. 但是knownhosts中有ip地址信息. 各不相同
   操作方式
4. 本地生成秘钥对
5. 本地和本地互联, 会在本地生成knownhosts, 本地也有authorized_keys
6. 把本地的.ssh目录整个复制到其余主机

```shell
# 在本地主机上
ssh-keygen -t rsa
ssh-copy-id xiao@127.0.0.1 # 连接自己
ssh-copy-id xiao@10.0.0.12 # 在authorized_keys 中增加一条记录, 让别人连接自己用
scp -r .ssh xiao@10.0.0.13: # 把自己的ssh配置都发给被的主机

# 在别的主机上
ssh xiao@10.0.0.12 # 实现免密访问
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
/dev/nvme0n1p2   10G  6.5G  3.6G  65% / # 这里是根目录
tmpfs           179M  4.0K  179M   1% /run/user/1001

```

回到家目录的方法

```shell
cd
cd ~
cd $HOME
```

Create User

- root的家目录在 `/root`
- 普通用户的家目录在 `/home/username`
  `/etc/default/useradd`

```shell
# 不会创建家目录
useradd <username>
# 会自动创建一个和用户同名的家目录 在/home下
useradd -m <username> # 同时会在家目录下创建 .bash_logout .bash_profile .bashrc 三个文件
userdel -r <username> # 删除用户, 并删除家目录
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
usermod -g <username> <groupname>
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

# Switch User

```shell
su - user # 完全切换用户
sudo ls /root # 临时切换, 有效时间为5分钟

vim /etc/sudores # 可以看到有sudo命令的执行权限的组
```

- visudo
- 直接修改 `/etc/sudoers`
- 把配置文件放入`/etc/sudoers.d`

### 给用户增加sudo权限

新增一个用户并给与sudo权限
检查wheel 组的权限

```shell
%用户组 主机(所有者:归属组) NOPASSWD:执行命令
用户   主机(所有者:归属组) NOPASSWD:执行命令 # 加上NOPASSW: 则不需要输入密码

# 测试 /etc/sudoers 语法
sudo visudo -c
```

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

查看系统信息

```shell

# red-hat, rocky
/etc/redhat-release
/etc/os-release


/etc/os-release
/etc/lsb-release

# 查看系统架构  x86
arch
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
- 文件夹读写执行的含义
  - 读 - 列出文件列表. 但是看不到 inode, 和文件的 meta 信息
  - 写 - 在目录下建立新的文件, 或者删除文件
  - 执行 - 看不到文件夹列表, 但是可以看到里面的文件的内容和属性, 如果你知道文件名
  - 所以一般要能访问目录, 一般会给 rx
- 如果只相对文件夹内的所有子文件夹加 x 权限, 对文件不加 x 权限, 可以使用 X 选项

```shell
# 查看文件类型, 可以看出是不是zip文件, 我
file <filename>
stats <filename> # 查看inode信息
[xiao@localhost ~]$ stat test.txt
  文件：test.txt
  大小：9               块：8          IO 块：4096   普通文件
设备：fd02h/64770d      Inode：448         硬链接：1
权限：(0644/-rw-r--r--)  Uid：( 1000/    xiao)   Gid：( 1000/    xiao)
环境：unconfined_u:object_r:user_home_t:s0
最近访问：2026-01-15 19:51:01.383704397 +0100
最近更改：2026-01-15 19:51:17.350441802 +0100
最近改动：2026-01-15 19:51:17.350441802 +0100
创建时间：2026-01-15 19:51:01.383704397 +0100
```

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
- sticky bit `t` 用于目录. 如果用户用这个目录的写权限， 该用户或者root只能删除和重命名他自己创建的文件

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
# 如果删除了 chmod 的可执行权限, chmod +x 也就不能使用了
chmod -x "which chmod"
# 此时就要用 acl把权限加回来
setfacl -m u:root:rwx /usr/bin/chmod
chmod +x /usr/bin/chmod
```

#### 文件属性中的selinux标签

- selinux 在安装完成后, 默认是启用的, 在生产中一般是禁用的

```shell
# 查看 Selinux 是否开启
getenforce
enforcing # 表示开启


setenforce # 临时设置selinux

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

# 重启才能生效
reboot
# 重启后检查
getenforce
Disabled
```

关闭firewalld

```shell
sudo systemctl stop firewalld

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

echo -e "\33[43;37m 黄底白字 \33[0m" # 这里 \33[ 和 \33[0m 是固定的
echo -e "\e[43;37m 黄底白字 \e[0m" # 这里用 \e 代替 \33
echo "PS1='\[\e[1;35m][\u@\h \W]\\$\[\e[0m\]'" >> .bashrc
```

# Shell Scripting

- Shell 是命令解释器
- Shell 命令的逐行执行的
- 干预脚本执行的过程
  - 通过循环, 函数, 条件判断
  - 通过用户输入
  - 通过信号

#### Shell 脚本的执行方式

```shell
# 在脚本开头增加shbang
#!/bin/bash

# 完成脚本编辑后, 需要给脚本增加执行权限
file.sh # chmod +x file.sh

# 执行方式
./file.sh # 通过文件名直接执行, 如果已经有执行的权限
/bin/bash file.sh # 执行shell命令来执行, 无论是否有执行权限, 推荐

source file.sh # 常用于加载环境变量
/bin/bash -x file.sh # 调试, 推荐
/bin/bash -n file.sh # 检查语法错误
/bin/bash -v file.sh # 先显示脚本内容, 再显示语法错误
# 带参数执行
/bin/bash /path/to/file arg1 arg2 arg3
```

#### 干预执行过程

- 获取用户输入

```shell
# 获取用户输入
read -p "please input your name" username # 把用户输入保存在username
```

#### 通过信号来干预脚本运行

以下命令也是信号

```shell
# 终止进程
ctrl + c
# 在后台挂起, stop的状态
ctrl + z
# 查看后台挂起的任务
jobs
fg num # 重新执行后台挂起的任务
kill %1 # 杀死挂起的进程

kill -9 pid # 杀死进程
```

### 环境变量

```shell
# 查看环境变量
echo $env_var
env
declare # 用于数组

# 环境变量的分类
# 普通环境变量
# 1. 普通样式环境变量 - 范围是当前终端, 终端没了就没了
变量名=变量值 # 变量值范围中不允许出现特殊符号
变量名='变量值' # 特殊符号会按照源字符输出
变量名="变量值" # 可以解析 变量

# 2. 命令样式的环境变量
变量名=`命令`
变量名=$(命令)
# 全局环境变量
export 变量名=变量值 # 声明为全局变量, 在env中有, 属于环境变量了
# 删除环境变量
unset 变量名

# 系统级别的变量
/etc/profile
/etc/profile.d/xxx.sh
/etc/bashrc # 系统用户级别
# 在上面几个文件定制环境变量 export var=value

# 内置变量
man bash # 有说明
$0 # 当前执行的shell脚本文件名
$1 # 第一个参数, 第二个参数是$2
$# # 当前shell命令的参数总数
$? # 获取上一个指令的返回值 0为成功, 非零为失败
$@ # 所有参数组成的列表
# 字符串
${ver_name:其实位置:截取长度}
${ver_name:0-5:5} # 从倒数第5个数开始,取5个
${#ver_name} # 字符串的长度

# 用户级别
~/.bashrc
~/.bash_profile

# 局部变量
local var=value

内置环境变量 - bash
$SHELL # 使用哪一种 Shell
$PS1 # 用户登录后的提示符
$PATH # 外部命令的搜索路径
$PS2 # 输入命令时的提示符变化 >

# 获取用户输入
read -p "please input your name" username
```

## Shell 语法

- 分号的使用, 普通的linux命令和条件判断后面加分号

```shell
$[]
let
expr
echo "2+3" | bc

[ 1==1 ] # 判断两个字符串是否相等
[ 1!=1 ] # 判断两个字符不相等
[ -z $string ] # 判断字符串是否为空
[ -n "$string" ] # 判断字符串是否为空
[ -d bak ] # 看是否是目录存在
[ -f file ] # 查看文件是否存在
[ -x file ] # 查看文件是否有执行权限
[ 1 -lt 11 ] # 小于
[ 1 -eq 1 ] # 判断数字

[ $# -eq 2] && exit # 如果变量个数为2, 就执行, 不等于2就退出
对条件

[ 条件1 -a 条件2 ] # and 单条件判断
[ 条件1 -o 条件2 ] # or
[[ 条件1 && 条件2 ]] # and 多条件判断
[[ 条件1 || 条件2 ]] # or
# 双方括号支持正在表达式

[[ $string==v* ]] # 变量匹配v开头的任意字符

read -s # 输入密码看不到
read -t30 # 30秒无输入, 退出

if []; then
	do
elif []; then
	do
else
	do
fi

case "${var}" in
 "start")
 	do;;
 "stop")
 	do;;
 *)
 	do;;
 esac

# 多行注释
:<<!
content
!

单位时间段里有多少个进程在运行,是并发 (1分钟能有多少个进程在运行)
在单位时间点有多少个进程在运行, 是并行 (一个核心一般只有一个进程在运行)

开机自动启动的进程成为守护进程
工作在工作空间的进程称为用户进程
工作在内核中的称为系统进程

创建进程的两种方式
fork
clone
	- 定制性参数

pstree -p  # 1号进程是systemd 或者叫 init(Centos7之前)

ls /proc/进程号 # 里面exe是主程序, status 是进程的状态
```

内部命令: Shell 中包含的指令. 随着 shell 加载到内存已经加载在内存中
外部命令: 有独立的磁盘文件, 需要从磁盘中读取的命令, 理论上说速度慢

> 有的命令可能即是内部命令, 优势外部命令.

### Loops in shell 循环

#### `for` loop

```shell
# 通过命令返回获得列表
for i in $(ls); do
	echo $i
done

# 手动构建列表
tasks=("ls -a", "df -h", "free -m", "uptime", "who")
for task in "${tasks[@]}"; do
	echo "Available commands: $task"
done

# 所有参数组成的列表
for arg in "$@"; do
	echo "${arg}"
done

# 构建序列
for i in {11..15}
do
	ping -c1 10.0.0.$i >> /dev/null 2>&1 && echo "10.0.0.$i is alive" || echo "10.0.0.$i is unreachable"
done

# 构建序列
for i in $(seq 2 2 10); do # 从2到10, 步长为2
	echo $i
done
```

#### `while` loop

```shell

num=1
while [ $num -le 6 ] # 如果小于6就执行
do
	echo "$(date +%F-%T)"
	sleep 0.5
	let num+=1
done
```

#### `until` loop

```shell
num=1
until [ $num -gt 5 ] # 如果大于5就停止
do
	echo "$num"
	sleep 0.5
	let num+=1
done
```

退出循环

```shell
exit # 退出整个脚本
break # 退出当前的循环
continue # 调到一下个
```

#### Shell function 传参

```shell
# 函数传参
# 如果脚本本身会被传递参数, 需要规避$1 在函数和脚本参数中混淆
name=$1 # 通过定义name来接受脚本的参数
say_hello() {
	local name="$1" # 在函数内部使用local 修饰的变量, 不会影响外部定义的name
	echo "hello world $name" # 调用函数内部的参数
}
say_hello "$name" # 向函数传入脚本的参数
```

#### Shell function 返回值

- 函数的状态返回值
- `$?` 函数体内部最后一条命令的执行结果的状态返回值
- `return` 自定以状态返回值 0-255, 只能是数字
- `echo` 可以用过来传递字符串

```shell
# 使用echo返回的字符串
get_os_type() {
 source /etc/os-release # 加载系统环境变量
 echo "$ID" # 这里函数返回环境变量
}
os_type=$(get_os_type)
[ "${os_type}"=="ubuntu" ] && cmd_type='apt' || cmd_type='yum'
echo $cmd_type
```

进程通信的方式

- 管道符
- 信号
- 共享内容
- socket

命令优先级

- 优先级越高的进程, 系统会默认多分配一些时间分片
- 用户自己的程序的优先级 -20 到 19 , 默认情况下是0
- 有系统进程的优先级0-100

```shell
# 查看当前进程优先级
root@ubuntu24-13:~# ps axo pid,cmd,nice | head
    PID CMD                          NI
      1 /sbin/init splash             0
      2 [kthreadd]                    0
      3 [pool_workqueue_release]      0
      4 [kworker/R-rcu_g]           -20
      5 [kworker/R-rcu_p]           -20
      6 [kworker/R-slub_]           -20
      7 [kworker/R-netns]           -20
     11 [kworker/u256:0-ext4-rsv-co   0
     12 [kworker/R-mm_pe]           -20

# 修改进程优先级
nice -n -9 ping 8.8.8.8

```

socket

- 对于网络来说, 进程是有port端口来标识的. 也就说ip:port定义了进程的接口. 这个ip:port的组合在内部抽象成一个socket, 也就是说我们只需要和socket通信, 即使ip:port变动了
  内存泄漏
- 进程执行完了, 为进程开辟的资源没有收回来. 可用的内存空间少了, 该收回来的没有收回来,就是内存的泄漏
  内存溢出 OOM out of memory, 内存不足
- 原本内存是需要20M内存, 但是实际使用超20M, 比如无限循环, 就是溢出. 造成程序死亡

```shell
lsof # 查看进程打开的文件
lsof -Pti :80 # 查看在使用80端口的进程
lsof | head # 列出所有被打开的文件

# 查看哪个程序在使用指定文件
root@ubuntu24-13:~# lsof /usr/sbin/nginx
COMMAND  PID     USER  FD   TYPE DEVICE SIZE/OFF    NODE NAME
nginx   1140     root txt    REG    8,2  1313752 8273989 /usr/sbin/nginx
nginx   1141 www-data txt    REG    8,2  1313752 8273989 /usr/sbin/nginx
nginx   1142 www-data txt    REG    8,2  1313752 8273989 /usr/sbin/nginx

```

杀死进程

```shell
kill -9 pid # 杀死单个进程
killall
pkilll # 连个命令都可以根据进程的名字来删除

# 在后端运行
sleep 1000 & # 如果终端关闭, 进程也会关闭
jobs 3 # 查看扔到后台的进程

nohup sleep 1000 >>/dev/null 2>&1 & # 不和终端绑定

ctrl + z # 可以把正在前台执行的程序扔到后台
bg # 扔到后面的进程, 可以通过bg查看进程号
fg %1# 把扔到后台的进程再拿到前台来, 这里1 是任务列表的序号, 这里%可以省略
kill %1 # 通过任务需要把进程干掉, 注意这个时候通过进程ID是干不掉这个进程的

uptime # 查看开机多久了
w # 查看所有登录的用户
```

并行执行

```shell
f1.sh&
f2.sh&
f3.sh&
wait
# 测试并行的需要多少时间
time /bin/bash bing.sh
```

定时任务 - 守护进程

- atd 一次性服务
- crond 周期行任务
  - 系统级别的周期性任务, 需要增加个人标识
  - 用户级别的周期性服务

```shell
# 一次性任务
apt install at
systemctl status atd # 查看守护进程

dpkg -L at # 查看工具安装的所有目录
/var/spool/cron/atjobs # ubuntu手工定制好的任务在这里
/var/spool/cron # rocky 的任务在这里
/etc/at.deny # 黑名单

at now + 5 minutes
at now + 1 hour
at now + 3 day

at 02:00 2016-09-20
ctrl + D # 退出, 执行命令内容保存在上面的目录里
at -l # 查看任务
at -d 2 # 删除任务, 2 是任务的编号
apt install crond

```

cron

```shell
/etc/crontab  # 主配置文件
/etc/cron.d # 子配置文件

* * * * *  # 每分钟执行一次
1 2 * * *  # 每天凌晨2点01分执行
*/5 * * * # 每5分钟
1-5 2-6 * * * # 每天的2-6时, 第1到第5分钟, 每分钟一次


crontab # 命令级别的周期任务
vim /etc/crontab # 全局级别的任务
/var/log/cron # 如果有时间定制的冲突, 会有日志
crontab -l # 查看全局的任务
crontab -u username -l # 查看指定用户的任务
crontab -e # 用户级别的定时任务
```

命令的执行顺序： Shell 会先查看命令别名， 然后内存中寻找内部命令, 如果找不到, 最后就在环境变量 Path 中定义的目录中找

```shell
\ls # 原始的 ls 命令
ls # 其实是别名 ls --color=auto
```

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

### Associate Array

类似于python的字典

```shell
declare -A http_code
http_code["200"]="OK - The request succeed"
http_code["404"]="Not Found - The requested resource could not be found"
http_code["500"]="Internal Server Error - The server encountered an error"
http_code["403"]="Forbidden - The server understood but refuses to authorize"
http_code["301"]="Moved Permanently - The resource has been moved"

read -p "Enter a http status code (type all to view every code) " code
if [[ "$code" == "all" ]]; then
	echo "All HTTP status and descriptions:"
	for key in "${!http_code[@]}"; do
		echo "HTTP $key: ${http_code[$key]}"
	done
elif [[ -n "${http_code[$code]}" ]];then
	echo "HTTP $code: ${http_code[$code]}"
else
	echo "Unknown HTTP status code: $code"
fi
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
whereis rm
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
# 查看时区
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
# 查询主机地址
hostname -I
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


# 查看分区信息
df -h
cat /proc/partitions

# 查询内核版本
uname -r
# 查询架构
uname -p
# 查询操作系统发行版本
cat /etc/os-release
lsb_release -a
```

# Motion 动作

i = inner
a= around
选中双引号中的所有字符 i"
选中双引号中的所有字符, 包括双引号本身 a"

选中光标所在的单词 iw
选中光标所在单词, 包含前后的空格 aw
选中小括号内的字符 i( 或者 ib
选中小括号内的字符包含括号本身 a(
选中花括号内的字符 i{ 或者 iB
选中 html tag 中的字符 it
is 句子
ip 段落

# Vim

```shell
# 复制粘贴
d # 删除选中内容
dd # 删除一行
5dd # 删除5行
5yy # 复制5行
5p # 粘贴5遍

# 行号
3,7
3,+5 # 后面5行
# 关键字
/str/,/stb/
,7 # 空代表光标所在行
% # 代表全文
```

```shell
G # 文末
gg # 文首

vim filename +6 # 定位到指定行数
vim filename +/sag/ # 定为到关键词

# 搜索
/
# 搜索到之后, n 或者/加回传 往下走 N, 或者?加回车往上走
:set cul # 辅助线
:set nu # 显示行号
# 撤销
u
```

## Visual 模式

```shell
v # 进入可视化模式, 选中单个字符
V # 进入可视化模式, 选中整行

ctrl + v # 纵向光标

viw # 选中一个单词
vib # 选中括号中的内容
```

案例: 在选中行的行首快速插入#

```shell
20G # 光标移动到 20 行
ctrl + v
# 使用方向键选中多行
I # 进入插入模式
# # 输入井号键
ESC # 退出插入模式

# 此时#号被插入多行
```

案例: 删除多行注释

```shell
20G # 光标移动到 20 行
ctrl + v
# 使用方向键选中多行
d # 直接删除
```

### 分屏

```shell
ctrl + w + v # 左右分屏
ctrl + w + s # 上线分屏
ctrl + w # 在两个分屏之间切换

ctrl + w + q # 删除当前窗口
ctrl +w + o # 删除所有窗口
```

## 扩展命令模式

```shell
:r <filename> # 将文件内容读到当前文件中
:w <filename> # 另存为
!command # 执行命令
r!command # 读入命令的输出
```

删除

```shell
:2d # 删除第二行
:2,5d # 删除第二行到第五行
:.,$d # 删除当前行到结尾
```

复制

```shell
:2,4y # 复制2到 4 行
y # 复制
p # 粘贴到光标的行
P # 粘贴到光标的前一行

:30:r /etc/isse # 在第 30 行读入一个新文件的内容
```

搜索替代

```shell
:%s/root/ROOT/ # 全文同一行只替代第一个
:%s/root/ROOT/g # 全文全局替换
5,12 s/root/ROOT/ # 在一定范围内替换

# 在每一行的行首加#
:$s/^/#/
```

锚定符号

```shell
^ # 行首
$ # 行位
G # 文末
gg # 文首
:8 # 第8行

```

加行号

```shell
:set nu # 显示行号
:set nonu # 不显示行号
```

通过修改 vim 的配置文件加行号

```shell
# /etc/vimrc 全局修改
# ~/.vimrc # 个人的配置

# 在文件中加入
set nu

```

忽略大小写

```shell
:set ic # igonrecase
:set noic
```

缩进

```shell
:set ai # autoindent 自动缩进 默认是 8 个
:set noai
:set ts=2 # 这是tab=2个字符的长度
:set et # 用空格代替tab, 只对修改增加的tab键会转换为空格

:set shiftwidth=4 # 配置缩进为 4 个字符
>> 缩进
<< 反缩进
```

保留格式

```shell
:set paste
```

显示不可见字符

```shell
:set list # tab键是 ^I
```

高亮

```shell
:set hlsearch
:set nohl # 取消高亮
```

语法高亮

```shell
:set syntax
```

把 windows 文件转换为 linux 文件

- vim会把 windows 格式识别为 dos 文件
- 使用 `hexdump -C win.txt` 可以看到 `0d 0a`, 如果是 linux 文件不会看到 `0d`

```shell
:set ff=unix # 转换为 linux 格式
:set ff=dos # 转换为 windows 格式
```

tab 用空格替代

```shell
:set et # extend tab 把 tab 转换为空格, 默认是 8 个空格
:set ts=4 # 一个 tab 键=4 个空格
```

加横线标记

```shell
:set cul # cursorline
```

加密

```shell
:set key=<yourpass>
:set key= # 清空密码
```

### CUT

```shell
cut -d" " -f5 # 取第5列
```

### sort

```shell
sort -u # 去除重复行
sort -r 3 # 降序
sort -n # 以数字排序
```

### uniq

```shell
uniq text.txt # 去重连续的数字
uniq -i text.txt # 大小写不明感
uniq -c text.txt # 计数
```

# SED

- 逐行对文件内容进行操作

```shell
sed 选项参数 数据来源
sed '1p' text.txt # 打印整个文件, 然后重复打印第一行
sed -n '5p' test.txt # 只打印第5行
sed -n '1p;3p' test.txt # 只打印第一行和第三行
sed -n '1,3p' test.txt # 打印第一行到第三行

# 编辑内容, 默认情况下不对文件进行操作
sed -i
sed -a
sed '3d' file # 删除第三行
sed '/str/d' file # 删除str所在行
sed '3,6d' file # 删除第三行到第六行, 不修改源文件
sed -i '3,6d' file # 删除第三行到第六行, 修改源文件
sed -i '/nologin/d' file # 删除 nologin所在的行

sed -i '2a\append_new_content after 2nd line' file
sed -i '4i\insert new content before 4th line' file
sed -i '3,6a\add new line to each line' file # 每一行前面增加一行内容

# 替换
sed -i 's/sed/SED/' file # 替换指定文字, 只替换一行中的第一个位置
sed -i '2s/sed/SED' file # 只替换第二行
sed -i 's/sed/SED/g' file # 全文更改
ifconfig ens33 | sed -nE 's/^\s*inet\s(.*) net.*/\1/p' # p是打印

```

# AWK

1. 逐行读取, 格式化输出
2. 默认情况下, 按空格切割成多列

```shell
awk '范围{print $列数}' sed.txt # 最后一列 $NF, 列数是从1开始的
awk '{print $(NF-1)}' sed.txt # 倒数第二列
awk '{print $1,$3}' sed.txt # 第一列和第三列, 使用默认符号拼接(空格)
awk '/sed2/{print $0}' sed.txt # 打印整行
# 拼接
awk '{print $1"---"$3}' sed.txt # 使用特殊符号拼接

# 处理行
awk 'NR==2{print $1"--"$3}' sed.txt # NR==2 表示第二行
awk '/keyword/{print $1"--"$3}' sed.txt # 定位到关键词所在行

# 设置分隔符
awk -F ':' '{print $7}' root.txt

# 前置动作, 核心动作, 后置动作
awk 'BEGIN{FS=":"}{print $2}END{print "ok"}' root.txt
```

# grep

- 文本处理三剑客 grep, sed, awk
- `grep <word> stdin` 如果没有带文件, 那就是用来接收标准输入的
- 多用于处理标准输出, 而不是文件

```shell
# 使用管道符
ifconfig eth0 | grep netmask

# 使用标准输入重定向
grep root < /etc/passwd

# 只看前三个结果
grep -m3 nologin /etc/passwd

# 不区分大小写
grep -i ROOT /etc/passwd
grep -in root /etc/passwd # 结果显示行号
grep -nir root / # 查询目录中递归

# 取反或者过滤
grep -v nologin /etc/passwd
grep -v "#" /etc/fstab # 过滤掉注释行

# 统计出现的次数
grep -c root /etc/passwd

# 只显示匹配到的字符串
grep -o root /etc/passwd

grep '^ssh' passwd # 查看ssh开头的行
grep 'in$' passwd # 查看in结尾的行

# quiet mode, 与 $? 连用, 判断上一个命令是否有返回
grep -q root /etc/passwd
if [ $? -eq 0 ]; then # 如果上一条命令返回是0, 则表示成功
    echo "Command succeeded"
else
    echo "Command failed"
fi

# 了解关联行信息
# 后续行数
grep -A3 root /etc/passwd # 在 root 后面的 3 行, After
# 前多少行
grep -nB3 root /etc/passwd # Before 并列出行号
# 前后多少行
grep -C3 root /etc/passwd

# 包含多个关键词, 或者关系
grep -e root -e bash /etc/passwd
# 包含多个关键词, 并且, 使用两个命令的组合
grep root /etc/passwd | grep bash

# 如果把过滤条件放入文件(每一行为一个条件, 条件之间是或的关系), 使用这个文件过滤另一个文件.
grep -f test.txt /etc/passwd
# 快速找出两个文件的相同之处
grep -f a.txt b.txt

# 递归查询文件内容, 但不处理软连接
grep -r root /etc/* # 查询所有文件中的内容
# # 递归查询文件内容, 处理软连接
grep -R root /etc/*


# 使用命令的结果来搜索
grep `whoami` /etc/passwd
grep "$USER" /etc/passwd

# 扩展的正则表达式
grep -E
grep -Ev '^$|#' keepalived.conf # 查看非空行,也非注释
egrep # 扩展的正则表达式, 这个是上面命令的简写
egrep -v "^#|^*" # 多个条件

# 组合
grep server1.zoo.cfg | sed -r "s/(.*)=(.*):(.*):(.*)/\1/" # 把找到的内容, 替换为第一个匹配到的内容

# 特殊符号
a? # 0或1次a
a* # 0或很多次a
a{n,m} # n到m次
a[,m] # 最多m次
a[n,] # 至少n次
a+ # 至少1次
```

# Storage

磁盘类型

- SCSI 早期
- SAS 是企业级存储, 家用对标的协议的是 SATA

### 硬盘分区

- 企业级硬盘一般在 15000 转
  默认情况下, 硬盘的命名为 `sda`, `sdb`, `sdc`延续下去
  一个硬盘下, 有多个分区. 记为 `sda1`, `sda2`, `sda3`
- ext 协议有 lost+found

```shell
ls /dev # 也可以看到字符设备
# 设备
vda, vdb # 在云环境上的虚拟硬盘
sda, sdb # 机械硬盘
nvmea, nvmeb # 固态盘
/dev/null # 黑洞
/dev/zero

fdisk -l | grep dev # 查看盘符

df # 分区, 文件系统等信息
du # 目录的存储空间
dd # 定制文件, 仅仅是测试用
lsblk #
```

```shell
# 1. 查看所有硬盘信息, 包括插在设备上的 U盘. 这些硬盘对应文件 /dev/xvdb
lsblk -f # 查看硬盘和分区的关系
# 2. 在硬盘上创建分区
fdisk /dev/sda
n # 创建新的分区
p # 主分区， 其他选默认
w # 保存
# 3. 更新分区表
partprobe
# 4. 在分区上创建文件系统, 格式化
mkfs -t ext4 /dev/xvdb1
# 或者, 格式化成功后才能看到uuid
mkfs.ext4 /dev/sdb1
mkfs.ext4 -f /dev/sdb1 # 强制格式化, 原来已经被格式化的硬盘
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 5242624 4k blocks and 1310720 inodes
Filesystem UUID: cfc24908-a8dc-4889-a7d5-867f46cee83a
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

# 验证格式化成功, 看uuid
[root@localhost ~]# lsblk  -f
NAME        FSTYPE      FSVER            LABEL                UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
sda
└─sda1      ext4        1.0                                   cfc24908-a8dc-4889-a7d5-867f46cee83a
sr0         iso9660     Joliet Extension Rocky-9-4-x86_64-dvd 2024-05-05-01-12-25-00
nvme0n1
├─nvme0n1p1 xfs                                               99cb0ca4-fd80-4a51-b3ea-12444114719d    656.7M    32% /boot
└─nvme0n1p2 LVM2_member LVM2 001                              C2dNrC-XyBX-zFFx-sFKu-KpJW-wbrS-9ihn1T
  ├─rl-root xfs                                               c93be473-47b7-4861-af33-9135d6b368b7     65.3G     7% /
  ├─rl-swap swap        1                                     c6936202-414a-4610-b57d-02ac62fca83f                  [SWAP]
  └─rl-home xfs                                               c9d94182-0c7c-42b5-8c96-64381f171310    124.1G     1% /home

# 挂载
mount -t 文件系统类型 /dev/sda1 /path/to/dir
# 或者推荐使用
mount /dev/sda1 /path/to/dir

# 查看现在被挂在这的目录
mount

# 卸载
umount /dev/sdb1
umount /path/to/dir

# 永久挂载, 在os启动时,自动挂载
[root@localhost ~]# blkid # 查询格式化后的信息uuid
/dev/mapper/rl-swap: UUID="c6936202-414a-4610-b57d-02ac62fca83f" TYPE="swap"
/dev/nvme0n1p1: UUID="99cb0ca4-fd80-4a51-b3ea-12444114719d" TYPE="xfs" PARTUUID="1d6de9ac-01"
/dev/nvme0n1p2: UUID="C2dNrC-XyBX-zFFx-sFKu-KpJW-wbrS-9ihn1T" TYPE="LVM2_member" PARTUUID="1d6de9ac-02"
/dev/sdb1: UUID="16848623-6880-406e-a8fe-7a7a8a914764" TYPE="xfs" PARTUUID="695c9a0f-01"
/dev/sr0: UUID="2024-05-05-01-12-25-00" LABEL="Rocky-9-4-x86_64-dvd" TYPE="iso9660" PTUUID="849e8820" PTTYPE="dos"
/dev/mapper/rl-home: UUID="c9d94182-0c7c-42b5-8c96-64381f171310" TYPE="xfs"
/dev/mapper/rl-root: UUID="c93be473-47b7-4861-af33-9135d6b368b7" TYPE="xfs"
/dev/sda1: UUID="cfc24908-a8dc-4889-a7d5-867f46cee83a" TYPE="ext4" PARTUUID="b6412df1-01"

# 修改 /etc/fstab
# defaults 表示默认挂载方式 0 表示是否需要每天备份, 第二个0 要不要自检
/dev/sda1     /mnt                   ext4     defaults        0 0
/dev/sdb1     /mnt                   xfs     defaults        0 0

mount -a # 重新mount


```

硬盘处理三个步骤

1. 设备分区
2. 创建文件系统
3. 挂载新的文件系统
   分区的类别
4. MBR, Master Boot Record 早先的方式, 局限性在单个硬盘不能超过 2T, 表示为dos
   1. 分区分为 主分区(最多 4 个), 1 个扩展分区(扩展分区里面再可以分逻辑分区).
   2. 主分区和扩展分区加起来不能超过 4 个
5. GPT, 可以支持 128 个分区, 容量可以达到 8Z (T, P, E, Z, B, Y) 1. 本身会自动备份分区表. ubuntu 上默认使用 GPT
   BIOS 和 UEFI

- BIOS Basic Input Output System 用于完成硬件的自检和操作系统的引导. 如果硬件自检有问题, 会有报警的声音. 在主板上, 仅仅支持 MBR, 空间只有 1M 内存, 只支持英语
- EFI 是Intel 开发的, 后来标准规范化之后用 UEFI, 可以支持图形界面, 硬盘分区支持 GPT, 支持多国语言. 当前的笔记本都是支持 UEFI
- 注意 BIOS + MBR 与 UEFI+ GPT 配合

```shell
# 查看分区
fdisk -l /dev/sda # 看到 Disklabel type: dos 就是 MBR 分区
hexdump -C -n 512 # 看前 512 /dev/sda

dd if=/dev/sda of=/data/mbr bs=1 count=64 skip=446 # 从源文件的 447 开始读 64 个字节
hexdemp -C /data/mbr
# 分区表一定是异地备份, 否则硬盘损坏也是看不了的

# 破坏分区表
dd if=/dev/zero of=/dev/sda bs=1 count=64 seek=446 # 从目标文件的 447 开始修改
dd if=/dev/zero of=/dev/sda bs=1M count=1 # 抹除分区表
```

分区工具

- fdisk 管理 MBR 分区
- gdisk 管理 GPT 分区
- parted 高级分区操作, 可以是交互和非交互方式. 很危险不建议用
- 可能遇到分区没有马上生效, 需要重启服务器. 此时使用 `partprobe` 把内存和硬盘的分区表进行同步

```shell
# mbr 分区
fdisk /dev/sdb
# 进入交互式界面
m # 帮助
p # 查看现有的分区
n # 创建分区
d # 删除分区
w # 存盘退出
q # 不存盘退出

# 创建了3个主分区和1个扩展分区. 在扩展分区里面有2个逻辑分区
Device     Boot   Start      End  Sectors Size Id Type
/dev/sda1          2048  2099199  2097152   1G 83 Linux
/dev/sda2       2099200  4196351  2097152   1G 83 Linux
/dev/sda3       4196352  6293503  2097152   1G 83 Linux
/dev/sda4       6293504 41943039 35649536  17G  5 Extended
/dev/sda5       6295552  8392703  2097152   1G 83 Linux
/dev/sda6       8394752 10491903  2097152   1G 83 Linux


```

### 文件系统

文件系统就是组织和管理文件的一种方式

- 文件放在哪里: 数据的存储
- 文件在哪里获取: 文件的查找
- 文件本身: 文件的属性, 权限
  linux中常见的文件系统
- xfs 支持的硬盘和分区空间更大, 支持 8E, 不支持文件系统的缩减, 只能扩展
- ext4 支持 1E, 支持缩减. ubuntu 偏好
- iso9660 只光盘特殊的文件系统
- swap
  windows 使用的文件系统
- FAT32
- NTFS
- exFAT

```shell
ls /lib/modules/`uname -r`/kernel/fs # 查看内核中支持的文件系统

df -Th # 查看当前文件系统
Filesystem                        Type   Size  Used Avail Use% Mounted on
tmpfs                             tmpfs  387M  1.2M  386M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv ext4    97G  6.3G   86G   7% /
tmpfs                             tmpfs  1.9G     0  1.9G   0% /dev/shm
tmpfs                             tmpfs  5.0M     0  5.0M   0% /run/lock
/dev/sda2                         ext4   2.0G  186M  1.7G  11% /boot
tmpfs                             tmpfs  387M   12K  387M   1% /run/user/1000

```

创建文件系统

- 也就是 windows 中常说的格式化. 会有文件的元数据存储的地方
- MBR 不能在扩展分区上创建文件系统. 只能在逻辑分区上创建
- 断电可能造成文件系统破坏
- 块是存放文件的最小单位. 一个文件至少得占一个块
- 块的大小可能是 1K, 2K, 4K, 取决于分区的大小. 如果分区只有 100M, 那么块可能只有 1K
- 保留块默认是 5%, 预留给 root 使用, 防止硬盘被普通用户写满

```shell
lsblk -f # 查看文件系统

mkfs.ext4 /dev/sdb1
# 创建完文件系统后, 就会产生一个 UUID
blkid # 可以用来查看所有 UUID
blkid /dev/xvdb1
# /dev/xvdb1: UUID="fa1e16c0-8c76-42c3-9816-bceda1a21fb8" TYPE="ext4"

# 创建目录用于挂载文件
mkdir /logs
mount /dev/sdb1 /logs # 这是临时性的挂载, 重启失效
mount  -o ro /dev/sdb1 /logs # -o 表示挂载选项, ro 表示可读可写
# 取消挂载
umount /logs

# 查看 ext4 文件系统的属性
tune2fs -l /dev/sdb1
# 查看 xfs 文件系统的属性
xfs_info /mysql
```

挂载分区

```shell
# 挂载持久化
/etc/fstab
# 添加下面的命令
UUID=fa1e16c0-8c76-42c3-9816-bceda1a21fb8 /app  ext4    defaults.grpquota       1       2
# defaults.grpquota 是挂载选项
# 第一个数字表示多长时间做一个文件系统的备份
# 第二个数字表示开机是否文件系统完整性检测 0 表示不检测, 1 表示先检测分局, 2 表示 文件系统 1 检测完了, 再检测

mount -a # 会触发读取 /etc/fstab 文件中新增加的行, 不能改变挂载的 option
# 如果用使挂载 option 变化生效, 需要重新挂载
mount -o remount /mysql

# 8. 加载所有新的文件系统 -t = type
mount -t ext4 /dev/xvdb1 /app
# 9. 修改目录所属的组
chgrp app /app

```

### swap 文件系统

- 一种虚拟内存技术, 因为性能问题, 建议禁用
- 安装 k8s 的时候, 要禁用 swap. 因为硬盘性能太低, 不能满足 k8s 性能要求

```shell
# 查看内存, 用以判断要使用多少swap
free -h
```

```shell
# 永久禁用 swap
# 在 /etc/fstab 中把 swap 的一行注释掉
# -i 表示in place edit 在原有的文件上修改
# .bak 表示将原来的文件做一个.bak的备份
# /swap 表示搜索含有 swap 字符的行
# /s 表示 substitution 替换 sed 's/pattern/replacement/flags'
# @^/@#@ 表示把行首替换为# 即注释改行
# /etc/fstab 表示要操作的文件
sed -i.bak `/swap/s@^@#@` /etc/fstab
```

```shell
# 临时禁用所有 swap
swapoff -a
free -m # 查看是否禁用
# 启用 swap
swapon -a
# 查看本机 swap
swapon -s # 实际上看到的是 /proc/swaps
# 创建 swap 文件系统
mkswap /dev/sdc1

# 修改 swap 的优先级 /etc/fstab
UUID=... swap swap defaults,pri=100 0 0
swapoff -a # 禁用 swap
swapon -a

# 删除 swap
# 先删除 /etc/fstab 中的 swap 的行
swapoff /dev/sdc1 3


# 创建文件形式的 swap
dd if=/dev/zero of=/swapfile bs=1M count=2048
# 格式化文件变成 swap 类型
mkswap /swapfile
blkid /swapfile # 这种情况下, 因为文件太多,设备重启时可能找不到 uuid
# 修改 /etc/fstab 使用文件名, 而不是 UUID
/swapfile      none      swap    default     0 0
chmod 644 /swapfile
swapon -a # 使生效
swapon -s # 查看

# 迁移 swap 文件
swapoff /swapfile # 禁用 swap
mv /swapfile /dtat/
# 修改 /etc/fstab
/etc/swapfile      none      swap    default     0 0
swapon -a # 启用

# 修改使用 swap 的阈值, 值越高表示倾向于使用swap, 值越低表示不使用swap
cat /proc/sys/vm/swappiness # 注意, 这不是一个文件. 没有文件的实体
echo 0 > /proc/sys/vm/swappiness # 可以通过这种方式来修改

sysctl -a | grep swappiness # 查看阈值
vim /etc/sysctl.conf # 在文件中添加
vm.swappiness = 30 # 可以修改为 0
sysctl -p # 使生效
```

### 光盘

- 光盘已经有文件系统, 所以只要挂载就能用了

```shell
sr0 # 光盘的设备名

# 手工挂载
mount /dev/sr0 /mnt
ls /mnt

# 自动挂载, 可以查看光盘里的内容
ls /misc/cd
# 实现自动挂载需要安装软件
yum -y install autofs
systemctl enable --now autofs

```

### U盘

- 不需要分区, 自动识别出一个设备名, 直接挂载
- 注意 NTFS Rocky 是不识别的, 无法挂载

```shell
lsblik # 查看 U 盘的盘符
sudo mkdir -p /mnt/usb
# 把优盘挂载在 mnt/usb下
sudo mount /dev/sdb1 /mnt/usb #
# 查看 U 盘插入后的日志

tail -f /var/log/messages
modinfo ntfs # ubuntu 查看文件系统是否支持, 有 NTFS 驱动

```

### du df

```shell
# du 查看文件夹的大小, 及文件夹的子目录, 以 k 为单位
du -sh /etc/ # -s 表示汇总,  查看目录的大小

# df 是查看文件系统大小
df -h
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
for i in {0..32}; do echo '- - -' > /sys/class/scsi_host/host$i/scan; done

lsblk # 查看新的硬盘是否出现

# 定义别名来实现3个命令
alias scandisk="echo '- - -' > /sys/class/scsi_host/host0/scan;echo '- - -' > /sys/class/scsi_host/host1/scan;echo '- - -' > /sys/class/scsi_host/host2/scan"

```

## LVM 虚拟卷管理

- 分区不能动态调整大小的, 需要删除重建. 会影响业务
- 避免单点故障

### RAID

- 如果服务器做了 RAID, 那么所有的物理硬盘就会变成一个逻辑上的磁盘. 这个逻辑上的磁盘也就是虚拟卷LV
- RAID可以做在软件层面和硬件层面. RAID的概念
- Mirroring 数据被完全的复制
- Striping 数据被放在多个磁盘中
- Parity 专用的校验数据和恢复数据, 可能使用独立的磁盘来存储
- RAID 扩容还是需要停服务, 关机

#### RAID0

- 2 块硬盘以上
- No redundancy, 磁盘的利用率最高
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
- RAID4 是 Block level parity check, dedicated parity disk, 但是防止校验位的硬盘经常坏
  ![[Pasted image 20250210065459.png]]

#### RAID5

- 至少要3个硬盘. 最多可以允许一个硬盘坏了. 当时重构数据需要花很长的时间
- 在安防监控中经常使用RAID
- no dedicated parity disk
- 在 3 块硬盘的情况下, 可用空间是 2/3 因为一块磁盘需要做 parity
  ![[Pasted image 20250210070043.png]]

#### RAID6

- 至少要 4 块硬盘
- 类似于RAID5, 可以是有两个Parity Drive. 最多可以允许坏两个disk
- 读性能增加, 写性能减弱. 因为有两个parity drive要写入.
  ![[Pasted image 20250210070205.png]]

#### RAID10

- 推荐使用 RAID 10
- 是RAID0和RAID1的结合. 至少需要4个盘.
- 先把两个硬盘组成 raid 1
- 再把两组硬盘组成 raid 0

#### RAID01

- 先把两个硬盘组成 raid 0
- 在把两个组硬盘组成 raid 1

## LVM 逻辑卷

- 是一种软件技术, 可以用来动态扩缩容
- 首先为每一个 Linux 块设备(分区或者硬盘)分配一个物理卷的标签. 和 RAID 不同的是, 块设备可以是不同大小的
- 由多个物理卷组成一个虚拟卷组 Volume Group
- VG Volume group 虚拟卷组, 这个组里面可以有多个虚拟卷. 每个卷如果磁盘的分区, 是相互独立的. PV是可以加到VG里, 实现扩缩容的
- LV Logical volume 虚拟卷. 这个就是独立的分区了, 可以挂载到目录下. 注意LV是可以动态扩缩容的
- 在生产中, 对根目录扩容是比较常见的
  ![[Pasted image 20250316073317.png]]
  分区的类型
- 8e Linux LVM
- 83 普通分区
- 82 SWAP
- PE 表示物理卷的盘区, 给逻辑卷分配的最小单位, 在创建卷组的时候指定

```shell
# 安装 LVM 工具
yum -y install lvm2

# 命令汇总
pvs
vps
lvs
pvdisplay
vgdisplay rl
lvdisplay /dev/rl/home
pvcreate # pvcreate /dev/sd{c,d,e}1
vgcreate # vgcreate -s 16M testvg /dev/sd{a,b}1
lvcreate # lvcreate -l 100 -n lv1 testvg # 100个PE块, 名字是lv1
pvremove
vgremove
lvremove
lvscan # 查看所有lv

# 扩容, 先扩lv, 在扩文件系统
vgextend testvg /dev/sdd1 # 给vg添加pv 分两步
lvextend -L +1G /dev/testvg/lv1 # 给testvg的lv1扩容1个G, 还需在文件系统中增加
resize2fs /dev/testvg/lv1 # lv已经增加了1G, 在文件系统中要resize
# 把所有VG所有的给lv, 一步到位
lvresize -r -l +100%FREE /dev/testvg/lv1

# 缩容, 先缩文件系统, 在缩lv
umount /dev/testvg/lv1 # 必须要退出 /mnt1 挂载的目录才能做umount
e2fsck -f /dev/testvg/lv1 # 检查文件系统
resize2fs /dev/testvg/lv1 3G # 文件系统缩容到3G
lvreduce -L 3G /dev/testvg/lv1 # lv缩容到3G
mount /dev/testvg/lv1 /mnt1 # 重新挂载

# 卸载
umount /dev/testvg/lv1
lvremove /dev/testvg/lv1
lvremove /dev/testvg/lv2
lvremove /dev/testvg/lv3
lvremove /dev/testvg/lv4
lvs
vgremove testvg
pvremove /dev/sd{a..e}1

# lv的路径格式
/dev/vg名字/lv名字
lvdisplay # 查看lv详情
[root@localhost ~]# lvdisplay /dev/rl/home
  --- Logical volume ---
  LV Path                /dev/rl/home
  LV Name                home
  VG Name                rl
  LV UUID                NuZ61m-kZpM-clRR-GU6d-fiUo-rvjD-JHSVRJ
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2026-01-17 11:55:46 +0100
  LV Status              available
  # open                 1
  LV Size                <125.08 GiB
  Current LE             32020
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:2


# 创建物理卷
pvcreate /dev/sdb1 /dev/sdc # 这个命令可以单独敲
pvs # 查看物理卷
pvdisplay # 查看物理卷详情

# 创建卷组
vgcreate -s 16M vg_name /dev/sd{b1,c}  # -s 指定 PE 大小为 16M
# 查看 vg
vgs
[root@localhost ~]# vgdisplay testvg
  --- Volume group ---
  VG Name               testvg
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <39.97 GiB
  PE Size               16.00 MiB
  Total PE              2558
  Alloc PE / Size       0 / 0
  Free  PE / Size       2558 / <39.97 GiB
  VG UUID               7zm1sf-dIfN-d4IV-CWAe-GVHZ-Y6MF-inewiL


# 创建逻辑卷
lvcreate -n <lv_name> -L 6G <vg_name> # 针对数据库进行扩容, -l表示 PE 的个数, -L 指定容量
lvcreate -n <lv_name> -l 20%free <vg_name>  # 从空闲的空间创建百分比
lvcreate -n <lv_name> -l 10%VG <vg_name> # 从VG的10%创建
# 查看逻辑卷
lvs
# 可以发现有三个名字对应逻辑卷
/dev/testvg/log_lv
/dev/mapper/testvg-log_lv
../dm-1

# 格式化
mkfs.ext4 /dev/testvg/mysql_lv
# 查看文件系统
blkid
# 提前创建挂载点文件夹爱
mkdir /log
mkdir /mysql
# 挂载文件系统
vim /etc/fstab
# 挂载
mount -a
# 查看挂载后的文件目录
df

```

测试逻辑卷性能

```shell
# 有缓存机制, 每次执行会变得更快, 性能不差的
dd if=/dev/zero of=/mysql/test.img bs=1M count=1024

```

逻辑卷扩容

- 逻辑卷扩容完毕后, 还需重新加载文件系统

```shell
# 扩充的容量不能超过 vg 剩余的容量
lvextend -l +318 /dev/testvg/mysql_lv # 使用 vgs 查看剩余的 PE,
lvextend -l +50%free /dev/testvg/mysal_lv # 把剩余空间的 50%都用完

# 对文件系统进行扩容, 只支持 ext4
resize2fx /dev/testvg/mysql_lv
# 对文件系统进行扩容, 只支持 xfs
xfs_growfs /dev/testvg/log_lv

# 推荐, 根据现有的文件系统, 进行扩容
lvextend -r -l +50%free /dev/testvg/mysal_lv # -r 表示根据现有的文件系统扩容
```

扩展卷组

```shell
pvcreate /dev/sdb2
vgextend testvg /dev/sdb2
vgdisplay
lvextend -r -L 8G  /dev/testvg/log_lv # 把逻辑卷扩充为 8G, 如果是+8G表示在原来的文件系统上新增 8G
```

缩容

- 当硬盘盘位都装满了, 某些磁盘利用率低的场景
- 缩减有风险, 容易造成数据破坏, 建议先备份, 而且只能离线缩减. 需要把文件夹的挂载关系取消
- 只支持 ext4, 不支持 xfs

```shell

# 1. 取消挂载
umount /mysql
# 2. 做文件系统的完整性
df -hT # 查看文件系统的类型
fsck -f /dev/testvg/mysql_lv
# 3. 缩减文件系统
resize2fs /dev/testvg/mysql_lv 4G # 目标缩减到 4G
# 4. 缩减逻辑卷
lvreduce -L 4G /dev/testvg/mysql_lv
# 5. 重新挂载文件系统
mount -a  # 如果缩减模块了文件系统, 在这一步就会报错
# 尝试修复文件系统
fsck /dev/mapper/testvg-mysql_lv
# 因为缩容失败, 重新构建文件系统
mkfs.ext4 /dev/mapper/testvg-mysql_lv
# UUID 该表, 需要重新挂载
blkid # 查看 uuid
vim /etc/fstab
mount -a
```

逻辑卷快照

- 快照会在卷组中开辟一块区域
- 快照一开始不会保存数据. 只要在发现数据变动时, 才会把变动之前的数据保存在快照中
- 快照的大小取决于逻辑卷中文件的大小. 如果现有文件很小, 则快照可以也很小

```shell
# 创建快照
lvcreate -n mysql_snapshot -s -L 100M -p r /dev/testvg/mysql_lv  # -s 表示快照逻辑卷, -p r 表示设置成只读
# 查看快照, 会有 snopshot status 项
lvdisplay

# 把快照挂载到 /mnt
mount /dev/testvg/mysql_snapshot /mnt
ls /mnt # 看得到文件, 但是文件内容是在源逻辑卷中


# 恢复快照
# 取消两个磁盘的挂载
umount /mnt
umount /mysql
# 使用命令还原快照, 但是快照会消失
lvconvert --merge /dev/testvg/mysql_snapshot # 写快照名子
# 挂载回去
mount /dev/testvg/mysql_lv /mysql

# 手工拷贝
mount /dev/testvg/mysql_lv /mysql
mount /dev/testvt/mysql_snapshot /mnt
\cp /mnt/* /mysql/

# 删除快照
umount /mnt
lvremove /dev/testvtg/mysql_snapshot

```

拆除硬盘

```shell
# 查看物理卷是否被使用
pvdisplay # 查看 Allocated PE
# 查看 VG 的空间, 判断是否有足够的 PE
vgdisplay
# 把这儿磁盘搬迁到同一个 VG 中的别的磁盘上
pvmove /dev/sdb2
# 从 vg 中移除物理卷
vgreduce <vg_name  /dev/sdb2
pvremote /dev/sdb2
# 如果是一个孤立硬盘的话, 就是可以拆走了
```

删除逻辑卷

```shell
# 删除挂载
vim /etc/fstab
# 取出挂载
umount /mysql
umount /log
# 删除逻辑卷
lvremove /dev/testvg/mysql_lv
lvremote /dev/testvg/log
# 删除卷组
vgremove testvg
# 删除物理卷
pvremote /dev/sd{c,b1}
# 拆除硬盘

```

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

查看文件内容

```shell
cat -A <file> # 显示行结束符
cat -A test.txt
add test$

cat -n <file> # 显示行号
more
less
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
echo "CPU: \c" # \c表示不换行
echo -n "CPU: " # - 表示不换行


# \E[1;32m 表示绿色 \E[0m 表示关闭颜色
echo -e "\E[1;32mCPU: \E[0m\c" # 绿色

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
# 安装可以使用hexdump的工具, 用ascii嘛的风格来看字符
sudo apt install util-linux
hexdump -C text.txt # 可以查看文件, 同时显示字符和 16 进制表示
[xiao@localhost ~]$ hexdump -C test.txt
00000000  61 64 64 20 74 65 73 74  0a                       |add test.|
00000009

# installl dos2unit to convert windows file to linux
sudo apt install dos2unit
dos2unit file.txt
# 或者使用 notepad++ 配置 Unix(LF)

```

扩展符号

```shell
echo {1..10} # 10个数字
echo {10..1..2} # 10 8 6 4 2
echo {a..c}{1..3}
touch a{1..10}.log # 创建10 个文件
```

history
`~/.bash_history` 历史执行的命令记录, 目前终端连接中生成的命令, 没有输入进去

```shell
!s # 执行最近一次 s 开头的命令
!?con # 执行最近一次命令中包含 con 字符的命令
echo !^ # 前面一个命令的第一个参数
echo !$ # 前面一个命令的最后一个参数
echo !* # 前一个命令的全部参数
!! # 前一条命令
echo `!!` # 获得前一条命令的执行结果

# 快速获取前一条命令的参数 esc (松手)+ .
# ctrl + r # 倒过来查 history 中的命令
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
ctrl + y # 粘贴删除的内容

ctrl + s # 锁死屏幕, 屏幕不会显示
ctrl + q # 两次 解锁屏幕

esc + b # 移动到当前单词的开头
esc + f # 移动到当前单词的结尾
```

# Linux 文件系统

- `/bin`和 `/sbin` 的区别是, `/bin` 是普通用户可以还行, `/sbin` 是管理员才能执行的程序

```shell
# 以下是Linux的通用目录
/home # 所有用户的文件夹都放在 home 下面
/root # root 用户的家目录
# 系统目录
/etc # 相当于 windows 中的注册表, 存储 linux 所有程序的配置文件
/boot # 存放开机启动需要的程序, linux 内核也在里面, 只有12M
/sys # 保存在内存中的文件, 用于硬件
/lib # 库文件
# 与命令相关的目录, 一般都保存在$PATH中
/bin # 存储 cd, mv 等命令的的二进制文件, 比如 ls 命令, 就是/bin/ls 文件
/sbin # 系统管理员使用的命令的二进制文件
/usr/bin  # used for sharing files between users, 类似windows中的Program files
/usr/sbin
/usr/share # 共享的
# 与程序相关的
/run # 程序运行的时候的临时文件
/var/run
/proc # 吧保存在内存中的文件, 用于进程
# 经常变化的文件目录
/var/log # 日志等文件
/tmp # 用户和程序的临时文件, 重启后会自动清理
# 与设备相关的
/dev # 硬件设备文件, 硬盘
/media # usb 介质
/mnt # 光盘挂载的入口
```

`/dev`下有块设备, 字符设备
块设备, 比如硬盘. 一个块包含多个字节
字符设备, 一个字符一个字符为单位 比如 `/dev/zero`, /dev/pst/1

蓝色: 文件夹
绿色: 可执行程序
红色: 压缩文件
浅蓝色:链接
`/etc/DIR_COLORS` 定义

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
df -i # 只能看到完成挂载的文件系统
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
[:blank:] # 空格
[:space:] # 空格
[:lower:] # 所有小写字母你[[:lower:]]表示所有小写字母中取一个
[:upper:] # 左右大写字母 [[:upper:]]表示所有大写字母中取一个
[:digit:] # 数字


.* # 注意 表示包含所有.开头的隐藏文件, 且包含父目录.., 如果此时是在家目录下做操作, 则可能删除根目录

# 多个字符
{1..3} # 1,2,3
{1, 5} # 1, 5
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
rename .txt .txt.bak *.txt # 把所有的 *.txt 文件中的.txt 替换为.txt.bak
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

sleep 5000 & # 重定向到后台运行

# 标准输入的重定向
echo 2+3>bc.txt
bc <bc.txt # < 是标准输入的重定向, 把文件中的内容输入给 bc
# 从 1+2+...+100
seq -s+ 100 >bc.txt;bc<bc.txt
seq -s+ 100 | bc
# 或者使用重定向高级用法
# < 后面跟文件
# << 后面跟多行
# <<< 后面跟字符串作为标准输入
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
- **EOF后面不能包含空格, 否则会有语法错误**

```shell
# 在脚本场景中的定制配置文件, 且格式繁琐, 可以使用 cat eof 的方法
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

cat >> a.txt <<-EOF # - 可以用于取消最后的 EOF 前的tab 键, 不能取消空格
newline1
newline2
newlo3
	EOF
```

tee 的和cat 却别在于cat在重定向之后不会在终端输出, 但是tee会同时在终端输出. 用于观察之脚本执行的过程

```shell
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
tr -dc abc < filename # 除了 a管道bc, 其他都删除
tr -s abc < filename # 发现连续的 a, 连续的 b, 连续的 c, 压缩成一个
tr -s ' ' <filename # 压缩空格
df | tail -n +2 | tr -s ' ' | cut -d" " -f5 | tr -d %

```

管道符后面创建一个子进程

```shell
echo $BASHPID;{ echo $BASHPID;sleep 100; } | { echo $BASHPID;sleep 100; }
# 第二个没有打印出来, 因为第二个标准输出是管道符后面的标准输入, 但是管道符后面不需要标准输入, 收所以就把第二个标准输出给丢弃了.
# 只会打印出第一个进程, 和管道符后面的子进程
```

### 软链接和硬链接

Hard Link

- 把一个文件起另外一个新的名称, 节点编号是相同的, 文件是同一个. 两个硬链接是平等关系
- 删除其中一个, 并不会删除文件. 除非把所有的名字都删除了. 节点编号则会被回收, 文件才会被删除
- 删除一个文件后, 文件的数据并没有立即被清空. 利用专业的数据恢复软件, 文件可以被找回
- 创建硬链接之后, 文件的连接数会变化. 如果应该文件有多个硬链接, 连接数会大于 1
- 硬链接但是必须在同一个分区中. 不能跨分区不能创建硬链接
- 硬链接只支持文件, 不支持文件夹

```shell
ln target hardlink_name # 创建硬链接文件
```

Symbolic Link

- 软链接也称为符号链接. 只是保存去到目标文件夹的相对路径
- 在升级版本时, 可以把原来的软链接删除, 为 app 指向新的文件夹, 创建一个软连接.
- 删除软连接的方法, 只能删除软链接文件`rm -f app`, 不是删除软连接下的文件 `rm -rf app/`
- 创建软连接要么使用绝对路径, 或者相对于目标软链接的相对路径
- 删除源文件, 软连接失效
- 软连接可以跨设备, 跨分区

```shell
# Abosolute Symbolic Link
ln -s absulute symboliclink_name
# 读取连接文件
readlink
```

# 传输文件

```shell
scp file <target_ip>:<path>
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
# 查询某个配置文件属于哪个软件包
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

- 实时查找 find
- 离线查找 locate

### `locate`

- 搜索速度非常快, 需要首先构建文件索引数据库
- 搜索依赖于数据库

```shell
# Rocky
yum install -y mlocate
# ubuntu
apt install -y plocate

updatedb # 构建或者更新数据库
locate <filename> # 搜索文件
locate -r '\.conf$' # 支持基本正则表达式
```

### `find`

- 优势在于搜索条件丰富
- 直接搜索磁盘
- 默认就是递归搜索当前目录下的所有文件
-

```shell
# 指定文件名来查找
find / -name "ni*.txt"
find / -iname "ni*.txt"
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
find / -group <groupname>
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
find /dir/ -atime +30 -delete  # 30 天前触碰过的文件, 删除
find ./ -amin +5 # 5分钟以前访问的文件
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

# 动作进阶
find ./ -name "*.txt" -print0 # 输出不换行, 配合xargs
find ./ -name "*.txt" -exec cp {} {}-bak \;  # 对每个匹配的文件执行指定的命令



-delete # 删除匹配到的文件
-ls # ls -dils 的格式显示匹配的文件
find ./ -name "*.txt" -ls
457      4 -rw-r--r--   1 xiao     xiao           70 1月 16 21:46 ./test.txt
448      4 -rw-r--r--   1 xiao     xiao         2128 1月 16 22:45 ./test_passwd.txt
462      4 -rw-r--r--   1 xiao     xiao           22 1月 16 23:36 ./number.txt


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

find *.sh | xargs rename "sh" "SH"
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
find -type f -print0 | xargs -0 ls -l
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
tar -zcf file.tar.gz /etc # 把目录打包, 压缩成 gz 文件
tar -jcf file.tar.bz2 /etc # 调用 bzip2 来压缩, 需要先安装 bzip
tar -Jcf file.tar.xz /etc # 调用 xz 来压缩, 需要先安装 xz

tar - xf file.tar.xz -C <target_dir> # 通用解压 gz, bzip, xz 文件

```

拆分

```shell
split -b 1M -d filename filename_suffix # -d 表示数字小编号, 每个文件切成 1M
cat filename* > filename # 合并这些文件
```

# Package Management 软件管理

```shell
whereis ls # 查看命令的地址
whatis # 命令是什么, 一句话解释

# 命令精简信息
cmd -h
cmd --hel
help cmd
```

- 在rocky中使用 rpm 离线安装
  - yum 在线安装
  - dnf 现在用的多, 从rocky10后都改dnf
- ubuntu 使用deb, dpkg 离线安装
  - apt 在线安装

```shell
yum makecache # 更新软件源的缓存信息
# 软件源的缓存信息保存在 /etc/yum.conf 主配置文件, 基本不做修改
# /etc/yum.repos.d/xxx.repo 定制的软件源配置文件
vim /etc/yum.conf.d/rocky.repo

# 定制软件源 aliyun.repo
# 指定的路径必须能找到repodata
[baseos]
name=aliyun - BaseOS
baseurl=https://mirrors.aliyun.com/rockylinux/10/BaseOS/x86_64/os/
gpgcheck=0

[appstream]
name=aliyun - AppStream
baseurl=https://mirrors.aliyun.com/rockylinux/10/AppStream/x86_64/os/
gpgcheck=0

[xiao@localhost ~]$ yum makecache
Repository baseos is listed more than once in the configuration
Repository appstream is listed more than once in the configuration
aliyun - BaseOS                                                     860 kB/s | 7.6 MB     00:09
aliyun - AppStream                                                  593 kB/s | 2.1 MB     00:03
Rocky Linux 10 - Extras                                              15 kB/s | 5.9 kB     00:00
Metadata cache created

# 查看本机上有哪些仓库
yum repolist
# 可以查看到仓库里是否有这个包
[xiao@localhost ~]$ yum list | grep nginx

# 定制一个local的 repo
mount /dev/cdrom /mnt
vim local.repo

[baseos-local]
name=aliyun - BaseOS
baseurl=file:///mnt/BaseOS
gpgcheck=0

[appstream-local]
name=aliyun - AppStream
baseurl=file:///mnt/AppStream
gpgcheck=0

yum makecache
yum repolist
```

`/etc/apt/sources.list.d/ubuntu.sources` ubuntu 的库文件内容

```shell
Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg


## Ubuntu security updates. Aside from URIs and Suites,
## this should mirror your choices in the previous section.
Types: deb
URIs: http://ports.ubuntu.com/ubuntu-ports
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

keyrings 的存储地方 `/usr/share/keyrings/ubuntu-archive-keyring.gpg`

- 大量的软件可能是没有包的, 只是提供了源码, 需要编译安装
- ABI 应用程序的二进制接口. windows 和 linux 的二进制程序默认是不兼容的
  - ELF (Exectable and Linable Format) Linux
  - PE () Windows

```shell
# 查看软件信息
RPM qi xz


tar xf nginx-1.20.2.tar.gz -C /usr/local/src # 习惯是把源码解压到这个目录
cat `find -name "*.c"` | wc -l # 统计所有文件的行数
find -name "*.c" | xargs cat | wc -l

# 查看程序依赖的库
which ls
ldd /usr/bin/ls
```

包管理器

- 安装是二进制可执行命令
- rpm 是离线安装命令, 不能解决包的依赖关系, yum 可以
- 安装和卸载都有依赖关系
- 对于rocky来说还有epel软件源

```shell
# 下面这个命令不能解决依赖关系, 很少用, 多用于查询
rpm -ivh <package_path> # -h 安全进度条
rpm -evh # 卸载, 不太使用


# 查询软件是否已经成功装上
rpm -q <package_name>
rpm -ql vsftpd # 查看软件安装在哪些目录下
rpm -qf /etc/passwd # 查看文件属于哪个软件包
rpm -qi <package_name> # 查询软件的版本, 描述, 安装时间

# 反查某个已经安装的工具, 在哪个软件包里
[root@localhost ~]# whereis whoami
whoami: /usr/bin/whoami /usr/share/man/man1/whoami.1.gz
# 通过工具所在文件的路径来反查软件包的名字
[root@localhost ~]# rpm -qf /usr/bin/whoami
coreutils-9.5-6.el10.x86_64

# 查询没有安装的包
rpm -qpi <package_name>

rpm -q vstfd || yum vstfd
```

dpkg 用于ubuntu

```shell
dpkg -i # install
dpkg -l vsftpd # 查看是否安装
dpkg -r vsftpd # remove
dpkg -L vsftpd # 查看安装路径
dpkg -S /usr/bin/whoami # 根据命令路径查看包名

apt update # 更新源信息
apt clean # 清除之前的缓存
# 软件源
/etc/apt/sources.list # 主文件
/etc/apt/sources.list.d/xxx.list # 子文件, 可以不存在

# 在新的ubuntu中, 在用 /etc/apt/sources.list.d/ubuntu.sources 文件中编辑软件仓库
# 可以通过重命名来禁用后, 只使用/etc/apt/sources.list 来管理, 否则会造成两者冲突, 但是使用sources.list 是一种相对的传统的方式
apt search nginx
apt-cache madison nginx # 只看这一个软件的版本列表
apt remove appname # 不会基础依赖
apt purge appname # 会移除配置文件
apt purge appname* # 会移除配置文件
find / -name 软件名 | xargs rm -rf # 删除相关文件
```

镜像仓库

```shell
cd /etc/yum.repos.d/
Rocky-BaseOS.repo
# 阿里云镜像, 找到有 repodata 的文件夹(元数据)所在的文件夹
https://developer.aliyun.com/mirror/
# 创建的仓库必须是 repo 后缀的, 文件名任意, 必须在 /etc/repos.d 目录下

[base] # 仓库名
baseurl=https://mirrors.aliyun.com/centos/7/os/x86_64/
gpgcheck=0

# 列出现在仓库的列表
yum repolist #-v 看到每个仓库包的个数
yum list package

```

仓库

- 一般是需要 base 仓库, AppStream 仓库, extras(不太常用的包), PowerTools
- 如果一个包在 Appstream和 ngix 的仓库里都有, 则用 `yum list nginx`列出的是版本高的库

```shell
[BaseOS]
name=aliyun BaseOS
baseurl=https://mirros.aliyun.com/rockylinux/8/BaseOS/x86_64/os/
gpgcheck=1 # 可以省略
gpgkey=https://mirros.aliyun.com/rockylinux/RPM-GPG-KEY-rockyofficial

[Appsteam]
name=aliyun Appstream
baseurl=https://mirros.aliyun.com/rockylinux/8/AppStream/x86_64/os/
gpgkey=https://mirros.aliyun.com/rockylinux/RPM-GPG-KEY-rockyofficial

[extras]
name=aliyun Appstream
baseurl=https://mirros.aliyun.com/rockylinux/8/AppStream/x86_64/os/
gpgkey=https://mirros.aliyun.com/rockylinux/RPM-GPG-KEY-rockyofficial

[PowerTools]
name=aliyun Appstream
baseurl=https://mirros.aliyun.com/rockylinux/8/AppStream/x86_64/os/
gpgkey=https://mirros.aliyun.com/rockylinux/RPM-GPG-KEY-rockyofficial
enable=1 # 表示使用此仓库, 0 表示禁用

# 本地磁盘上也有一个路径看gpg
ls /etc/pki/rpm-gpg
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rockofficial
```

卸载

```shell
#  rocky8 之后会把依赖的包也卸载
yum remove nginx
```

下载包不安装

- 用于把文件和依赖复制到 u 盘中, 带去没有网络的地方安装包

```shell
# 默认只下载相关的包默认到 /var/cache/yum/x86_64/7/
yum install package --downloadonly -downloaddir=/data/httpd httpd
ls /data/httpd
# 复制到优盘后, 在新的服务器上
rpm -ivh /data/*.rpm # 使用文件安装
```

企业级仓库 epel

```shell
yum list epel* # 模糊查询 epel 包在哪个仓库
yum install -y epel-release # 会自动给你配好仓库
yum uninstall -y epel-release # 卸载自动配置的仓库

# 自己配置
[epel]
baseurl=https://mirros.aliyun.com/epel/8/Everything/x86_64
	   =https://备用 url

# 安装一个包
yum install sl -y

```

在公司内部搭建私有仓库服务器

检查安装好的程序, 有没有启动

1. 看端口
2. 查进程
3. 查服务
   1. rocky 系统, 软件安装完成后, 如果有服务的话, 默认不启动
   2. ubunut 系统, 默认是启动的

二进制包

- 安装方便, 管理不方便
  管理方便
- 源码包
- 安装不方便
- 安装后, 所有的文件都在一起

```shell
systemctl status appname # 查服务

# 安装编译环境
apt install build-essential gcc g++ libc6 libc6-dev libpcre3 libpcre3-dev libssl-dev libsystemd-dev zlib1g-dev
# 下载 nginx

# 使用 configure 配置安装文件, 生成Makefile文件, 并指定安装目录
./configure --prefix=/data/server/ngnix

# 编译软件
make
# 安装软件
make install

# 启动nginx
/data/server/nginx/sbin/nginx
# 检查进程看
ps aux | grep nginx
apt install curl
curl localhost

apt install bind9 # 包的名字
systemctl status named # 服务的名字

```

### systemctl

```shell
systemctl start
systemctl stop
systemctl restart named # 重启, 杀死旧的进程
systemctl reload named # 重载, 不杀死进程, 直接加载变动的配置, 使立刻生效
systemctl status ngnix
systemctl is-active nginx # 查看服务是否启动
systemctl enable # 设置开机自启动
systemctl is-enabled nginx # 查看是否设置为开机启动
systemctl disable nginx # 禁用开机启动
systemctl disable --now firewalld # 立刻关闭服务
systemctl cat nginx# 看服务文件内容

# 每个服务有专用的服务配置文件, 如果立刻要让systemctl知道, 等于上户口
systemctl daemon-reload

# 服务常见的路径
/etc/systemd/system
/usr/lib/systemd/system # 多用这个目录
/lib/systemd/system
# 服务文件的名字是用 xxx.service
# 服务文件内容: [unit], [service], [install]
root@ubuntu24-13:~/nginx-1.29.4# cat /etc/systemd/system/bind9.service
[Unit] # 描述性信息
Description=BIND Domain Name Server
Documentation=man:named(8)
After=network.target
Wants=nss-lookup.target
Before=nss-lookup.target

[Service] # 服务启动的配置信息
Type=notify
EnvironmentFile=-/etc/default/named
ExecStart=/usr/sbin/named -f $OPTIONS
ExecReload=/usr/sbin/rndc reload
ExecStop=/usr/sbin/rndc stop
Restart=on-failure

[Install] # 希望被谁管理
WantedBy=multi-user.target
Alias=bind9.service

# 定制自己的ngnix服务
vim /etc/systemd/system/nginx.service
[Unit]
Description=Nginx server
After=network.target

[Service]
ExecStart=/data/server/ngnix/sbin
ExecStop=/data/server/ngnix/sbin -s stop
ExecReload=/data/server/ngnix/sbin -s reload
Type=forking

[Install]
WantedBy=multi-user.target

# reload
systemctl daemon-reload

```

```shell
init 3 # 这里的3 是systemd 里面的服务的运行级别, 每个服务都是一个目标 target, 如何把服务分组管理, 每个小组都是运行级别, 这些级别就是0, 1, 2, 3,4, 5, 6, 其中 3是文字级别, 5是图形级别

# 图形级别 = 文字级别 + 图形服务
# 文字级别: multi-users 图形服务 graphical
```

# 网络

- 网络命令
- 网络高可用
- 网络测试

```shell
# 需要安装 net-tools 才能使用 mii-tool, netstat
apt install net-tools
# 新的默认命网络包时 iproute

# mii-tools 查看网卡状态
mii-tool ens33
ens33: negotiated 1000baseT-FD flow-control, link ok

# 查看网卡详细状态, 速率
ethtool ens33
ethtool -i ens33 # 查看网卡型号

# 查看端口号
netstat -tnulp | grep nginx
# 查看某个服务使用的端口号
grep ssh /etc/services

ifconfig ens33 # 查看具体网卡
ifconfig ens33 | grep netmask | awk '{print $2}' # 获取网卡的ip地址
ifconfig ens33 | awk '/netmask/{print $2}'

ifconfig ens33 11.0.1.17/24 # 临时设置IP地址

```

|          | net-tools | iproute    |
| -------- | --------- | ---------- |
| 网卡地址 | ifconfig  | ip address |
| 路由配置 | route     | ip route   |
| 端口信息 | netstat   | ss         |
|          |           |            |

### 修改网卡名

```shell
vim /etc/default/grub
# 增加 if.names=0
GRUB_CMDLINE_LINUX="crashkernel=auto resume=/dev/mapper/rl-swap rd.lvm.lv=rl/root rd.lvm.lv=rl/swap rhgb quiet net.ifnames=0"
# 需要执行后重启
grub2-mkconfig -o /etc/grub2.cfg; reboot
```

网络配置文件路径

```shell
# Rocky 10 网卡
cat /etc/NetworkManager/system-connections/ens160.nmconnection
[connection]
id=ens160
uuid=1476ba0b-2d0a-37d5-8a2d-17fab6207b7b
type=ethernet
autoconnect-priority=-999
interface-name=ens160
timestamp=1769069759

[ethernet]

[ipv4]
address1=11.0.1.12/24
dns=11.0.1.2;
gateway=11.0.1.2
method=manual

[ipv6]
addr-gen-mode=eui64
method=auto

[proxy]
```

Openeuler 网卡

```shell
# OpenEuler 和 Centos7
cat /etc/sysconfig/network-scripts/ifcfg-ens160
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=eui64
NAME=ens160
UUID=d937ef01-6a63-4760-95a0-f3eaad056718
DEVICE=ens160
ONBOOT=yes
IPADDR=11.0.1.14
PREFIX=24
GATEWAY=11.0.1.2
DNS1=11.0.1.2
```

ubuntu 网卡

```shell
# Ubuntu
以下3个文件保留一个就够了
/etc/netplan/50-cloud-init.yaml # 默认标准的
/etc/netplan/01-network-manager-all.yaml # 桌面版默认安装network manager 服务产生的
/etc/netplan/90-nmxxxx.yaml # 在桌面版中有图像页面配置IP地址的地方
```

在OpenEuler和Centos 7 中对新增的网卡进行静态IP
修改网卡名很麻烦, 一般是第一次标准化之后, 就不再修改

```shell
在虚拟机里添加一个新的网卡
nmcli device # 查看新设备添加的网卡
nmcli con # 这是network manager 服务配套的客户端工具, 查看网卡是否处于连接状态

systemctl restart NetworkManager # 重启服务, 这条命令等于下面两条命令
nmcli con reload # 加载网络配置
nmcli con up xxx # 启动连接

nmcli device up ens224 # 启动网卡, 启动网卡后, 才会有配置文件
# 修改配置文件
vim /etc/sysconfig/network-scripts/ifcfg-ens224
# 修改内容如下
BOOTPROTO=none
# 删除UUID, IPV6 # 可以通过直接复制现有网卡的配置文件来配置, 确保文件中没有uuid, 但是网卡名需要更具 nmcli device 中匹配
IPADDR=10.0.0.114
NETMASK=255.255.255.0 # 或者 PREFIX=24
DNS=10.0.0.2
GATEWAY=10.0.0.2
# 保存以上内容

nmcli device
DEVICE  TYPE      STATE                   CONNECTION
ens160  ethernet  connected               ens160
ens224  ethernet  connected (externally)  ens224 # 表面网卡不受管理
lo      loopback  connected (externally)  lo

nmcli device down ens224 # 关闭网卡
nmcli device set ens224 managed yes # make new NIC managed by NetworkManager
necli device up ens224 # 重启网卡
systemctl restart NetworkManager # 对于默认的网卡只需要重启配置文件就能生效
```

在Rocky 9和10里面管理网卡配置属性, Network Manager, 但是如果用复制现有网卡的方式, 不管用

```shell
vim /etc/NetworkManager/system-connections/ens160.nmconnection
# 下面是修改内容
[connection]
id=ens160
type=ethernet
interface-name=ens160

[ipv4]
address1=10.0.0.12/24,10.0.0.2 # 在一个网卡上可以定义多个IP地址, 这里表示第一个IP地址, 后面跟着的是路由网关
dns=10.0.0.2
method=manual # 手工配置. auto 表示dhcp
# 以上是修改内容


```

# ubuntu

默认使用netplan服务

```yaml

# 修改
vim /etc/netplan/50-cloud-init.yaml
# 以下是配置
network:
	ethernets:
        ens33:
            addresses:
            - 10.0.0.16/24
            nameservers:
                addresses:
                 - 10.0.0.2
                search: []
			routes:
			- to: default
			  via: 10.0.0.2
		ens37:
			addresses:
			- 10.0.0.116/24 # 增加新的ip地址
    version: 2
# 以上是修改的配置

# 修改完成后
netplan apply
```

网络命令

```shell
hostname -A # 显示所有的主机名
hostname xiao # 设置临时的主机名
vim /etc/hosts # 长久的主机名
10.0.0.12 rocky10-12.xiao.com xiao
vim /etc/hostname # 临时的主机名

hostname -I # 显示所有的网卡的IP地址
```

### 加上 IP 地址

```shell
ip a a 10.0.0.8/24 dev ens160
```

curl

```shell

curl -v www.google.com # 头部和内容都显示
curl -I www.google.com # 只显示头部, 可以看到服务是apache还是nginx
curl -X GET www.baidu.com # 定义方法
```

# DNS

- www 是主机名, google.com 是域名
- com 是二级域名, 根是点
- DNS 劫持, 使用httpdns 解决
- DNS 解析分为正向解析和逆向解析
- 如果我向一个DNS服务器做一次请求, 至少会拿到下面3条记录
  - A 记录
  - NS 记录
  - SOA 记录

正向解析记录

- A 记录, 解析成ipv4地址
- AAAA记录, 解析成IPv6 记录
- NS记录, 证明该服务器是域名解析服务器
- SOA记录, 所有记录的第一条, 记录的版本和保鲜时间
- CNAME 网站的别名
- MX 邮件场景
- SRV 通过关键词解析出某个服务
- TXT 其他文本

反向解析

- PTR 把IP地址解析成网站域名

```shell
# 解析查看, 下面三个命令的结果可能会受到host文件的影响
dig # 重要, 专业
nslookup # 和dig差不多
host www.jd.com
# 缓存查看


# ubuntu dns 服务器设定
/etc/netplan/90*
nameservers
# rocky
dns
# openeuler
/etc/sysconfig/network-scripts/ifcfg-ens224
dns1

# 临时设定 dns 服务器地址
/etc/resolv.conf

# 查看dns 地址信息, 整体查看, 解析查看
resolvectl # 在ubuntu中默认有 systemd-resolved 服务
# 在rocky10 中需要自己安装
yum list | grep systemd-resolved
systemctl start systemd-resolved
systemctl status systemd-resolved
resolvectl statistics # 查看缓存命中
resolvectl flush-caches # 清理缓存
resolvectl reset-statistics # 清理缓存

# systemd-resolved 使用的端口是53号
netstat -tulnp | grep systemd
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      836/systemd-resolve
udp        0      0 127.0.0.53:53           0.0.0.0:*                           836/systemd-resolve
cat /etc/resolv.conf # 这个是临时生效的, 会被systemd-resolve 接管
cat /etc/systemd/resolv # 定制全局的dns解析配置, 如果网卡和任何一个host文件都没有定义dns服务器时, 则会使用全局dns定制
# 修改完成后重启服务
systemctl restart systemd-resolved


# 在rocky9 有nscd 命令, rocky10上没有
apt install nscd
nscd -g # 查看缓存
nscd -i hosts # 清除缓存

# windows
ipconfig /display # 显示dns缓存
ipconfig /flushdns # 清理dns
# windows 上缓存的dns
c://windows/System32/drivers/etc/hosts
```

dig

```shell
dig google.com
dig google.com @8.8.8.8 # 定制使用8.8.8.8 来解析ip地址
dig -t A www.jd.com @114.114.114.114 # 指定记录类型
dig +trace # 查看多级域名解析
dig +short # 显示的内容少, 简洁
```

nslookup

```shell
nslookup www.google.com
nslookup www.google.com 114.114.114.114
```

host

```shell
host www.google.com
host www.jd.com 114.114.114.114
```

whois 类似站长工具

```shell
whois www.jd.com
```

## dns 软件 Bind

- 常见的有pind, knot, coredns等

```shell
# 在rocky selinux 环境中安装的话, 最好把防火墙和selinux关掉
# 关闭防火墙

# 关闭selinux


# 在rocky上安装
yum install bind
# 服务是named
systemctl status named
systemctl start named
systemctl enabled named # 设置为开机自启动
systemctl restart named # 修改配置后重启生效
netstat tunlp | grep named # 53, 如果可以看到自己的ip地址, 说明允许别的主机使用本地dns服务

rpm -ql bind # 查看软件包所在位置
/etc/named.conf

vim /etc/named.conf # 在里面看到的相对路径是/var/named下的
# 如果要允许别人来访问, 需要加上any
options {
	listen-on port 53 {127.0.0.1; any};
	allow-query {localhost; any;}
}
# 工作目录
/var/named

# 在ubunt上安装
apt install bind9
# 服务名 bind9 named
systemctl status bind9
systemctl status named
# 默认可以被外部访问
dpkg -L bind9
/etc/bind/named.conf # 主配置文件
/etc/bind/named.conf.options
/etc/bind/named.conf.default-zones # 里面有解析文件
# 程序内容
/var/cache/bind


```

查看日志

```shell
# rocky 下查看日志
tail /var/log/messages -f

```

数据解析文件

```shell

# 名称 TTL 类型 记录类型 数据内容
cat /etc/bind/db.local
;
; BIND data file for local loopback interface
;
$TTL    604800 # 全局的TTL
@       IN      SOA     localhost. root.localhost. (
                              2         ; Serial # 版本号一定变, 否则任务是旧数据
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      localhost.
@       IN      A       127.0.0.1
@       IN      AAAA    ::1

```

定制dns配置

1. 添加zone文件
2. 增加记录解析王建
3. 检测配置语法
4. 重启服务即可
5. dig命令解析测试
   修改zone文件

```shell
# 假设 11.0.1.13 是dns服务器, 在dns服务器上配置如下
# vim /etc/bind/named.conf.default-zones
# 如果二级域名 magedu.com
zone "magedu.com" {
        type master;
        file "/etc/bind/db.magedu.com"; # 指定record文件路径
};

# 创建也够record
vim db.magedu.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     magedu-dns. admin.magedu.com. (
                        20260204        ; Serial # 如果刚更新了记录, 必须需修改版本号
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               NS      dns1.
dns1            A       11.0.1.13 # 这是有dns服务的服务器
www             A       11.0.1.13 # 这是有web服务的服务器
*               A       11.0.1.199

# 检查zone文件的语法错误
named-checkconf
# 检查record文件是否正确
named-checkzone magedu.com db.magedu.com # 二级域名 record文件名
# 重启dns服务
systemctl restart named
# 如果是修改了record 则重载一下配置文件
systemctl reload

```

在客户机上配置

```shell
# 临时修改dns服务器

vim /etc/resolv.conf
# Generated by NetworkManager
nameserver 11.0.1.13

# 测试
dig magedu.com @11.0.1.13
; <<>> DiG 9.16.23-RH <<>> www.magedu.com @11.0.1.13
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17216
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 61fbe9da0ba47eb40100000069839c5b2c5a376ccac65f0d (good)
;; QUESTION SECTION:
;www.magedu.com.                        IN      A

;; ANSWER SECTION:
www.magedu.com.         604800  IN      A       11.0.1.13

;; Query time: 1 msec
;; SERVER: 11.0.1.13#53(11.0.1.13)
;; WHEN: Wed Feb 04 20:22:05 CET 2026
;; MSG SIZE  rcvd: 87

dig magedu.com @11.0.1.13
; <<>> DiG 9.16.23-RH <<>> xxx.magedu.com 11.0.1.13
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50074
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 854e3a7916fdfc4b0100000069839d876e0bff89bc33025c (good)
;; QUESTION SECTION:
;xxx.magedu.com.                        IN      A

;; ANSWER SECTION:
xxx.magedu.com.         604800  IN      A       11.0.1.199

;; Query time: 0 msec
;; SERVER: 11.0.1.13#53(11.0.1.13)
;; WHEN: Wed Feb 04 20:27:05 CET 2026
;; MSG SIZE  rcvd: 87

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 1100
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 854e3a7916fdfc4b0100000069839d876e0bff89bc33025c (good)
;; QUESTION SECTION:
;11.0.1.13.                     IN      A

;; Query time: 378 msec
;; SERVER: 11.0.1.13#53(11.0.1.13)
;; WHEN: Wed Feb 04 20:27:05 CET 2026
;; MSG SIZE  rcvd: 66


```

如果要增加备用dns节点

```shell
# 安装 bind
# 关闭防火墙
# 只需要修改zone文件,不需要添加record文件, 因为record文件会从主dns上拉取
vim /etc/named.rfc1912.zones
zone "magedu.com" IN {
	type slave
	masters {11.0.1.13;}; # 指向主的dns
	file "slaves/db.magedu.com"; # 把从主dns上备份下来的record存在下面的文件中
};

# 确保record的目录存在 /var/named/slaves
grep direct /etc/named.conf
        directory       "/var/named";


# 启动服务
systemctl start named
# 查看是否同步到记录
ll /etc/named/slaves

# 此时在客户机上配置主备dns
vim /etc/resolv.conf
nameserver 11.0.1.13
nameserver 11.0.1.15

# 如果dns记录需要变更, 需要主dns设备上修改
vim /etc/bind/db.magedu.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     magedu-dns. admin.magedu.com. (
                        20260206        ; Serial # 修改序列号
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

                NS      dns1
                NS      dns2 # 增加dns2 备份
dns1            A       11.0.1.13
dns2            A       11.0.1.15 # 指定备份 dns ip
www             A       11.0.1.14
image           A       11.0.1.114
*               A       11.0.1.199

# 更新完毕后reload服务
systemctl reload named
# 此时备用dns会自动下载新的record, 如果没有下载, 最终的解决方案是删掉备用设备上的/var/named/slaves/db.magedu.com 文件
```

反向解析 PTR记录, 给一个IP地址, 解析成域名

```shell
A 记录
blog.magedu.com. 86400 IN A 10.0.0.167
# PTR 反向的记录是, 把ip地址的位置颠倒
167.0.0.10.in-addr.arpa. 86400 IN PTR blog.magedu.com
```

dns 转发配置

1. 配置转发服务器的
2. 禁止加密验证
3. 被转发服务的防护墙关闭
   在 named.conf.options 修改

```shell
forwarders: {
  10.0.0.14;
};
forward first; #
dnssec-validation no; # 需要修改为no, 非常重要, 否则在主设备和被转发之间的设备之间会加密通信问题
# 以上是配置修改内容
named-checkconf
systemctl reload named
# 在本地测试转发请求
dig www.go-lang.magedu.com @localhost
# 排查故障 ubuntu 系统
tail -f /var/log/syslog
# 排查故障 rocky 系统
tail -f /var/log/messages

```

```shell
# bind 自己的状态工具
root@ubuntu2204-13:/etc/bind# rndc status
version: BIND 9.18.39-0ubuntu0.24.04.2-Ubuntu (Extended Support Version) <id:>
running on localhost: Linux x86_64 6.8.0-71-generic #71-Ubuntu SMP PREEMPT_DYNAMIC Tue Jul 22 16:52:38 UTC 2025
boot time: Sat, 07 Feb 2026 11:55:45 GMT
last configured: Sat, 07 Feb 2026 12:09:38 GMT
configuration file: /etc/bind/named.conf
CPUs found: 1
worker threads: 1
UDP listeners per interface: 1
number of zones: 104 (98 automatic)
debug level: 0
xfers running: 0
xfers deferred: 0
soa queries in progress: 0
query logging is OFF
recursive clients: 0/900/1000
tcp clients: 0/150
TCP high-water: 0
server is up and running

```

# CDN

内容分发网络 GSLB

- 能力: 网络分发能力, 内容分发能力
- 网络分发能力 智能DNS CNAME, ip地址设备
  - 一个域名解析出多个域名
  - 从客户的ip地址, 分析出距离最近的站点
    内容管理 - 网站平台
    CDN 的工作
- 刷新, 在活动结束, 清空数据, 减少垃圾数据
- 预热, 在活动前, 先将数据推送出去

通信级别安全

- 双方的通信链路是否安全 - 数据加密 - 对称加密算法 (大数据量)
- 服务器目标是否合法 我承认的 - 对称加密算法 (小数据)
- 从服务器里面获得的文件是否没有被修改 - 单向加密

对称加密算法: 加密和解密的秘钥是相同的
非对称加密算法: 有公钥和私钥
ssh 通信: 公钥加密, 私钥解密
身份验证: 私钥加密, 公钥解密

使用公钥加密, 容易被别人伪造数据

```shell
md5sum file # 计算md5值
sha256sum file
scp filename user@ip # 传递文件
```

# PKI

CA 证书

- CA是互联网上公认相信的中间人. 在操作系统中已经安装了常见的CA机构的公钥
- CA向我的公司的服务器颁发证书. 证书用CA的私钥进行加密, 即数字签名. 证书里面包含了CA的信息
- 用户在访问我公司的网站, 我的网站把CA机构发给我的证书, 传给用户. 用户拿着证书用CA的公钥进行解密, 确认是CA的签名, 那这网站就是合法的网站

CA机构

- CA机构分为两种. 一种是第三方公认的. 一种其实企业内部自建用于内部通信的
- CA机构机构自己颁发证书, 自己生产私钥

网站申请CA证书的路程

1. 网站生成秘钥对
2. 向CA机构发送证书请求
3. CA机构审核完毕后, 向服务器发放证书
4. 网站把证书放在指定的目录
5. 网站使用web服务器开启功能, 所有的数据传输都基于证书进行加密和解密
6. 当用户来访问网站的时候, 首先拿到的是证书.

PKI Public Key Infrastructure 公钥基础体系, 是一个包含硬件, 软件, 人员, 策略和规程的集合, 用于实现基于公钥密码体制的秘钥和证书的产生, 管理, 存储, 分发和撤销等功能
RA Registration Authority 注册审核机构
是CA的延伸, 负责数字证书的申请, 注册和注册工作. 它获取并认证用户的身份, 向CA提出证书请求, 并审核用户的申请资格
csr certificate signature request .csr 签名请求, .crt 证书
证书发布系统: 负责证书的发放和管理, 包括证书存储, 证书查询和证书的状态更新. 可以使用LDAP (Lightweight Directory Access Protocol) 等协议来发布和查询证书
证书库: 用于存储已颁发的数字证书和证书撤销列表(CRL)等消息. 用户可以通过证书库查询其他用户或实体的数字证书, 已验证其身份和合法性
KMC 密钥管理中心: 负责为CA系统提供密钥的生成, 保存, 备份, 更新, 恢复和查询等密钥服务. 解决分布式企业应用环境中大规模密码技术应用所带来的密钥管理问题
应用接口API: 为各种应用系统提供与PKI体系进行交互的接口, 使应用系统能够方便地使用PKI的安全服务, 如数字签名, 加密通信等

# OpenSSL

- OpenSSL 是传统意义上的做法, 在云原生领域, 则是使用cfssl 工具集

```shell
openssl version # 查看当前版本
openssl list --help # 查看命令

openssl md5 filename # 对文件hash

openssl passwd -d 加密的方式 密码的内容 -salt "random string" # -d 加密的方式

# openssl 生成秘钥对
# 生成私钥
openssl genrsa -out test.key
# 根据私钥信息, 生成公钥信息
openssl rsa -in test.key -pubout -out test.pubkey

```

CA 创建证书

```shell
# centos 系统
yum install openssl-libs
# centos 系统, 配置文件
cat /etc/pki/tls/openssl.cnf

# 证书的配置中, CA需要配置的内容
[ CA_default ]

dir		= /etc/pki/CA		# Where everything is kept
certs		= $dir/certs		# Where the issued certs are kept
crl_dir		= $dir/crl		# Where the issued crl are kept
database	= $dir/index.txt	# database index file.
#unique_subject	= no			# Set to 'no' to allow creation of
					# several certs with same subject.
new_certs_dir	= $dir/newcerts		# default place for new certs.

certificate	= $dir/cacert.pem 	# The CA certificate
serial		= $dir/serial 		# The current serial number
crlnumber	= $dir/crlnumber	# the current crl number
					# must be commented out to leave a V1 CRL
crl		= $dir/crl.pem 		# The current CRL
private_key	= $dir/private/cakey.pem # The private key

x509_extensions	= usr_cert		# The extensions to add to the cert
# 以上是 CA 配置内容

# 创建pki使用的目录
mkdir -pv /etc/pki/CA/{certs,crl,newcerts,private}
# 生成CA使用的私钥
cd /etc/pki/CA
openssl genrsa -out private/cakey.perm 2048
# 生成CA的证书
openssl req -new -x509 -key /etc/pki/CA/private/cakey.pem -days 3650 -out /etc/pki/CA/cacert.pem
# 查看证书需要特别的命令
openssl x509 -in /etc/pki/CA/cacert.pem -noout -text
# 生成一个索引文件
touch /etc/pki/CA/index.txt
# 生成一个16进制序列号
echo 0F > /etc/pki/CA/serial # 新的序列号是这之后10
# 签发证书, 假设已经收到服务器传来的证书
openssl ca -in /data/test.csr -out /etc/pki/CA/certs/test.crt -days 100
# 此时对于服务器的证书生成了
# 注意如果直接将这个证书发给windows设备, 这个证书还是不受信任的. 因为windows客户机上还没有将自建的CA机构的证书导入<受信任的根证书机构>
# 当成功导入CA证书后, 服务器的证书才是受信任的证书


# ubuntu 系统
apt install openssl libssl-dev
cat /etc/ssl/openssl.cnf

```

在服务器侧

```shell
# 首先创建一个目录
mkdir /data/
openssl genrsa -out /data/test.key # 创建一个私钥文件
# 生成签名请求
openssl req -new -key test.key -out test.csr

```

## Firewall 防火墙

- 在ubuntu中防火墙叫 ufw, 在rocky中叫 firewalld

```shell
# 查看被允许访问的服务. 如果里面没有http, 则不允许被访问80端口
firewall-cmd --list-services
cockpit dhcpv6-client http ssh

# 增加httpd服务
firewall-cmd --add-service=http --permanent
firewall-cmd --reload # 重启服务


# 查看防火墙服务是否启动
systemctl status firewalld
systemctl disable --now firewalld # 立刻关闭
getenforce # 查看selinux是否关闭了
```

查询防火墙服务

```shell
# ubuntu 中
ufw
# rocky 中
firewalld
```

# Nginx

```shell
dkpg -L nginx-common | grep index
# 在ubunt上的 nginx 的默认主页
/var/www/html/index
# 在rocky上的 nginx 默认主页
/usr/share/nginx/html/index.html

# 必须要确保服务器的防火墙开启http服务被访问, 否则客户系统访问会出现host或者没有路由的报错
firewall-cmd --list-services
firewall-cmd --add-service=http --permanent
```

Apache

```shell
# Rocky
yum install -y httpd
systemctl start httpd
systemctl status httpd
netstat -tulnp  # 查看80 端口是否开启


# 在防火墙中配置
firewall-cmd --add-service=http --permanent
firewall-cmd --reload # 重启服务

```

配置文件

```shell
# Rocky 上 软件包名 httpd
# web站点目录
/var/www/html # 目录权限属于 apache
# 主配置文件
/etc/httpd/conf/httpd.conf
# 子配置文件
/etc/httpd/conf.d/
# 服务名
httpd

# ubuntu上 apache2
apt install apache2
systemctl status apache2
# web 站点目录
/var/www/html # 这目录的权限 www-data
# 配置文件
/etc/apache2/apache2.conf
# 子配置文件
conf-xxx site-xxx
# 服务名
apache2

ps aux | grep apache # www-data
```

```shell
# 更新apache
apachectl -T

```

# 进程

```shell
# 查看进程解构, 包含线程
pstree -p # 查看进程树, 子进程
pstree username # 查看只属于某个用户的进程
pstree -p pid # 只看一个进程
pstree -p | grep 进程名 # 查询某个进程

# 查看进程状态,类似pstree
ps -auxf # a 表示所有终端中的进程, x 包括不连接终端的进程, u 进程所有者的信息, f 展示进程之间的关系

ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,comm | head # 自己定义进程的列
echo $BASHPID # 查看 bash 进程的 ID

# 看细节, 看哪个程序占用的资源多
top
top -n 1 # 刷新一次的数据
top -n 3 # 刷新3次数据
top -d 1 # 指定刷新频率为1秒, 默认为3秒
# top 的交互式命令, 只能按一次
P # cpu 排序
M # 内存排序
N # 进程降序
T # 时间排序
O %CPU>0.1# 根据关键排序


vmstat # 整体
vmstat -n 1 # 每秒刷新
iostat # 看输入输出 ubuntu有, rocky没有


```

### telnet

```shell
# 安装 telnet
yum install -y telnet

telnet youtube.com 443
### 如果连接正常反馈如下
Trying 142.250.190.206...
Connected to youtube.com.
Escape character is '^]'.
# 收入协议
GET /HTTP/1.0
# 返回页面
```

# 系统初始化

## Ubuntu

### 安装vmtools

```shell
sudo apt update -y
sudo apt instal open-vm-tools open-vm-tools-desktop -y
```

### 配置root ssh

- desktop 版本默认没有安装openssh-server - 默认情况下禁止root用户登录, 且没有为root用户设置密码
  安装openssh-server

```shell
sudo apt update -y
sudo apt install openssh-server
sudo systemctl status ssh
```

允许root用户登录. 修改 `/etc/ssh/sshd_config` 文件, 并重启 ssh服务

```shell
#PermitRootLogin prohibit-password
PermitRootLogin yes
```

root用户, 默认没有密码

```shell
sudo passwd root
```

重启ssh服务

```shell
sudo systemctl restart ssh
```

### 配置或调整图形界面

```shell
# 关闭图形界面
init 3
# 打开图形界面
init 5

# 禁用桌面图形方式启动
systemctl set-default multi-user.target
reboot

# 默认使用图形界面登录
systemctl set-default graphical.target
```

### 关机和重启

```shell
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

## Rocky

### Rocky 更改IP地址

```shell
hostnamectl set-hostname rocky9-15
exit  # 退出shell生效
exec /bin/bash # 重新创建一个终端也能生效
sudo nmcli con down ens160 # 断开网卡
# 修改配置文件
vim /etc/NetworkManager/system-connections/ens160.nmconnection

# 删除网卡的uuid, 时间戳, ipv6
[ipv4]
address1=11.0.1.15/24,11.0.1.2
dns=11.0.1.2
method=manual
# 修改完毕, 重启服务
systemctl restart NetworkManager
```

### 关闭selinux

```shell

# /etc/selinux/config
SELINUX=disabled

# shutdown firewall
systemctl stop firewalld
```

### 安装GUI

```shell
yum grouplist
yum groupinstall "带 GUI 的服务器"
sudo dnf update # dnf 是yum 的升级版本 或者 sudo yum update # 软件依赖升级
yum makecache # 更新软件源信息, 这里是安全的, 不会安装和更新任何包, 只有meta data的更新
```

2. 最小化安装
3. 关闭防火墙

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

# 内核

- 内核的参数只有查看和修改

```shell
# 内核参数
CONFIG_EXT4_FS=y # build-in 默认在启动的时候就加载
CONFIG_USB_SERIAL=m # 需要的时候才加载
CONFIG_SOME_FEATURE=n # disabled

```

```shell
lsmod # 查看当前安装的内核模块
modeinfo ipip # 查看模块信息, 依赖
insmod # 手工解决依赖
insmod /lib/modules/5.15.0-119-generic/kernel/net/ipv4/ip_tunnel.ko
insmod /lib/modules/5.15.0-119-generic/kernel/net/ipv4/tunnel4.ko
insmod /lib/modules/5.15.0-119-generic/kernel/net/ipv4/ipip.ko
rmmod ipip # 删除模块

modprobe ipip # 自动安装, 解决模块依赖的问题

# 管理linux内核参数
sysctl -a # 查看参数
root@ubuntu24-13:~# sysctl -a | grep ip_forward
net.ipv4.ip_forward = 0
net.ipv4.ip_forward_update_priority = 1
net.ipv4.ip_forward_use_pmtu = 0

内核参数以文件的形式存在 /proc/sys 目录中. /proc 和 /sys 是linux系统中的伪文件系统
临时编辑一个参数
echo 1 > /proc/sys/net/ipv4/ip_forward
sysctl -w 'net.ipv4.ip_forward=1' # 立刻临时生效

# 永久生效
vim /etc/sysctl.conf
vim /etc/sysctl.d/xxx.conf # 子文件
sysctl -p # 使主文件的修改立即永久生效
sysctl -p /etc/sysctl.d/net.conf # 使子文件的修改立即永久生效
reboot # 使修改立刻永久生效

```
