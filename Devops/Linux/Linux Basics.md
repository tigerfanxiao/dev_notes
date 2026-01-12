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
ctrl + alt + f3 # 切换到非 gui
ctrl + alt + f2 # 切换到 gui
```

# Linux SSH

### Install `openssh`
在服务器侧安装服务端
```shell
sudo -i
# for rocky 8
yum -y install openssh-server openssh-client
# for ubuntu or debian
apt-get install openssh-server openssh-client

systemctl start sshd # start ssh service 
systemctl status sshd # show ssh service status
```

远程登录服务器
注意: Ubuntu 不允许root账户远程登录, 需要用别的账户
```shell
ssh username@<ip>
```

使用公钥文件登录ssh
```bash
chown 400 perm # 400 means read
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
# 如果删除了 chmod 的可执行权限, chmod +x 也就不能使用了
chmod -x "which chmod"
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
$PS1 # 用户登录后的提示符
$PATH # 外部命令的搜索路径
$PS2 # 输入命令时的提示符变化 >
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

# Storage 存储
磁盘类型
- SCSI 早期
- SAS 是企业级存储, 家用对标的协议的是 SATA
### 硬盘分区
- 企业级硬盘一般在 15000 转
默认情况下, 硬盘的命名为 `sda`, `sdb`, `sdc`延续下去
一个硬盘下, 有多个分区. 记为 `sda1`, `sda2`, `sda3`
- ext 协议有 lost+found

```shell
# 1. 查看所有硬盘信息, 包括插在设备上的 U盘. 这些硬盘对应文件 /dev/xvdb
lsblk # 只是查看内存中的分区表
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

硬盘处理三个步骤
1. 设备分区
2. 创建文件系统
3. 挂载新的文件系统
分区的类别
1. MBR, Master Boot Record 早先的方式, 局限性在单个硬盘不能超过 2T
	1. 分区分为 主分区(最多 4 个),  1 个扩展分区(扩展分区里面再可以分逻辑分区). 
	2. 主分区和扩展分区加起来不能超过 4 个
2. GPT, 可以支持 128 个分区, 容量可以达到 8Z (T, P, E, Z, B, Y)
	1. 本身会自动备份分区表. ubuntu 上默认使用 GPT
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
p # 打印现有的分区
n # 创建
d # 删除
w # 存盘退出
q # 不存盘退出
```
### 文件系统
如果没有文件系统, 就没有文件的概念. 所以的数据就是 0 1 存放在硬盘上
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
- 虚拟内存使用
- 安装 k8s 的时候, 要禁用 swap. 因为硬盘性能太低, 不能满足 k8s 性能要求
- swap建议放在硬盘的最外圈或者 ssd
- 可以用分区的方式或者文件的方式当 swap 用, 但是分区的方式性能好
- 如果是一个文件形式的 swap 是可以迁移的
- swap 容量推荐
	- 系统内存低于 2G, RAM量的两倍
	- 系统 2GB-8GB, 等于 RAM 的量
	- 系统 8G-64G, 建议用 4G 到 RAM 的 0.5 倍
	- 超过 64G, 独立负载(至少 4GB)
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
# 禁用所有 swap
swapoff -a 
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

# 修改使用 swap 的阈值
cat /proc/sys/vm/swappiness
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
- 注意 NTFS  Rocky 是不识别的, 无法挂载
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

####  RAID0
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
- VG Volume group 虚拟卷组, 这个组里面可以有多个虚拟卷. 每个卷如果磁盘的分区, 是相互独立的
- LV Logical volume 虚拟卷. 这个就是独立的分区了
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

# 创建物理卷
pvcreate /dev/sdb1 /dev/sdc # 这个命令可以单独敲
pvs # 查看物理卷
pvdisplay # 查看物理卷详情

# 创建卷组
vgcreate -s 16M vg_name /dev/sd{b1,c}  # -s 指定 PE 大小为 16M
# 查看 vg
vgs

# 创建逻辑卷
lvcreate -n <lv_name> -L 6G <vg_name> # 针对数据库进行扩容, -l表示 PE 的个数, -L 指定容量
# 查看逻辑卷
lvdisplay
# 可以发现有三个名字对应逻辑卷
/dev/testvg/log_lv
/dev/mapper/testvg-log_lv
../dm-1

# 创建文件系统
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
### `locate`
- 搜索速度非常快, 需要首先构建文件索引数据库
- 搜索依赖于数据库
```shell
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
find /dir/ -atime +30 -delete  # 30 天前触碰过的文件, 删除
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
# Package Management
debien
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
 - rpm 命令不能解决包的依赖关系, yum 可以
 - 安装和卸载都有依赖关系
```shell
# 下面这个命令不能解决依赖关系, 很少用
rpm -ivh <package_path> # -h 安全进度条
# 查询软件是否已经成功装上
rpm -q <package_name>
# 查询软件的版本, 描述, 安装时间
rpm -qi <package_name>
# 查询没有安装的包
rpm -qpi <package_name>
rpm -ql <package_name> # 查询包里的文件
rpm -qf /etc/passwd # 查看文件属于哪个包
rpm -q vstfd || yum vstfd
rpm -e # 卸载, 不太使用

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

# 网络

```shell
# 需要安装 net-tools 才能使用 mii-tool
apt install net-tools
# mii-tools 查看网卡状态
mii-tool ens33
ens33: negotiated 1000baseT-FD flow-control, link ok

# 查看网卡详细状态, 速率
ethtool ens33 
ethtool -i ens33 # 查看网卡型号
```
### 修改网卡名
```shell
vim /etc/default/grub 
# 增加 if.names=0
GRUB_CMDLINE_LINUX="crashkernel=auto resume=/dev/mapper/rl-swap rd.lvm.lv=rl/root rd.lvm.lv=rl/swap rhgb quiet net.ifnames=0"
# 需要执行后重启
grub2-mkconfig -o /etc/grub2.cfg; reboot
```
### 加上 IP 地址

```shell
ip a a 10.0.0.8/24 dev ens160
```
### 固定IP
```shell
# find network interface card name is ens33
ip a
ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:3e:47:8c brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 11.0.1.131/24 metric 100 brd 11.0.1.255 scope global dynamic ens33
       valid_lft 1120sec preferred_lft 1120sec
    inet6 fe80::20c:29ff:fe3e:478c/64 scope link
       valid_lft forever preferred_lft forever

# find gateway is 11.0.1.2
xiao@ubuntu:~$ ip route
default via 11.0.1.2 dev ens33 proto dhcp src 11.0.1.131 metric 100


```


```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - 11.0.1.151/24 # fixed ip 
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
      routes:
        - to: default
          via: 11.0.1.2
```


```shell
# make the config of fixed ip effective
sudo netplan apply
```
# 进程

```shell
ps -aux 
pstree -p # 查看进程树, 子进程

echo $BASHPID # 查看 bash 进程的 ID


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
- desktop 版本默认没有安装openssh-server
	- 默认情况下禁止root用户登录, 且没有为root用户设置密码
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
sudo nmcli con down ens160 # 断开网卡
# 修改配置文件

```
修改配置文件
```shell
vim /etc/NetworkManager/system-connections/ens160.nmconnection

# 删除网卡的uuid
[ipv4]
address1=11.0.1.15/24,11.0.1.2
dns=11.0.1.2
method=manual
```
更改网卡配置
```shell
systemctl restart NetworkManager
```

### 安装GUI
```shell
yum grouplist
yum groupinstall "带 GUI 的服务器"
sudo dnf update # dnf 是yum 的升级版本
sudo yum update # 软件依赖升级

yum makecache # 更新软件源信息

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
