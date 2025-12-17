Nana devops repo
https://gitlab.com/twn-devops-bootcamp/latest/10-kubernetes

Ubuntu 22.04
```shell
# check Ubuntu version
lsb_release -a
cat /etc/os-release

uname -a

# 查看cpu
lscpu 
# 查看内存
lsmem
```
 root 用户切换
```shell
# switch to root without password
sudo su 
# switch to root with password of current user
sudo -i
# switch to root 
su - # specify the password of root user

exit # exit root 
```

user and group management
```shell
# 这两个命令是 interactive, user friendly
adduser <username>
addgroup <groupname> 
deluser

# low-level command, 可以使用在脚本中的
useradd <uername>
groupadd <groupname>
delgroup

# create user and add to the current group
useradd -G <groupname> <username>
# remove user from the group
gpasswd -d <username> <groupname>
```
Create app user for security best practice
- Create separate user for every application
- Give it only the permission it needs to run App
- Don't work with the Root user
```shell
# 在 root 账户下
adduser xiao
# add user to sudo group
# 使用sudo命令时, 输入是xiao的密码
# 注意, 使用sudo命令创建的文件的user owner 和 group owner都是root
usermod -aG sudo xiao # add xiao to sudo group
su - xiao # switch to xiao 

# 下面整个操作允许我本地电脑直接连接到 xiao的账户下, 而不是默认的root账户
# 因为这是某个droplex中的用户账户, 所以不能使用保存在digital ocean中的ssh-pubkey
# 复制我本地电脑的pub ssh key, 到远端设备的家目录
mkdir .ssh # 在xiao的家目录下创建 .ssh

vim .ssh/authorized_keys 
# 复制公钥, 并保存
# 此时本地电脑就可以使用xiao的账号来访问远端droplet, 在本地电脑输入
ssh xiao@<ip>
```

modify user
```shell
# change the primary group of user
usermod -g <primary_group> <username>
# set the user group list, replace the current one
usermod -G <primary_group>,<secondary_group> <username> 
# add secondary group to user
usermod -aG <secondary_group> <username> 

# show all the groups for current user logged in 
groups 
```

modify file owner
```shell
chown user:group filename
chgrp groupname filenaem # change only group owner for the file
```
文件权限
```shell
drwxrwxr-x 
- # regular file
d # folder
l # symboic 软连接

chmod g-w filename # remvoe the write permissino from group owner
chmod a+x filename # add permission for all users
chmod g=rwx filename # configure it directly
chmod 777 filename 
```

pipe, redicrect, stdin, stdout
```python
ls /usr/bin | less

ls /usr/bin > file.txt
ls /usr/bin >> file.txt # 追加

stdin 0
stdout 1
stderr 2

# 多个命令的组合
command;commend
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
- `/opt` 一般是用于非linux标准库中的第三方应用, 比如chrome, vscode等

# 用户管理
Linux 其实可以看做是三种用户, 一种是系统管理员root用户, 一种是普通用户, 一种是为应用创建的用户, 比如数据库管理员
```shell
sudo adduser <username>
su - username # 切换用户, 并进入该用户的家目录
```
# Package Management
apt-get 和 apt是两个不同的包管理工具. apt比apt-get更好用, apt more user-friendly output

```shell
# ubuntu 20.04
sudo apt-get update # find avaible package for your distro
sudo apt-get upgrade # install the available package

sudo apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/list*

apt intall <package_name>
# uninstall software
apt remove <package_name>

# 查看 repository, 增加的repository都会放在这个文件中
cat /etc/apt/sources.list 


```

ppa Personal Package Archive, 是指没有Linux 版本审核过的个人包, 有一定风险

# bash
首先在#!制定了执行.sh文件的shell. 因为在一个系统中可能存在多种shell, bash, zsh 等. 
然后个人通过创建文件方式创建的 .sh 文件一般没有执行权限. 通过
```shell
chmod u+x file.sh # 给当前用户增加执行权限
```
最后可以通过制定shell来运行脚本
```shell
bash file.sh
# 如果文件已经有了可执行权限
./file.sh
```
bash 的例子
```shell
#!/bin/bash
echo "Setup"
# 定义变量 
file_name=config.yaml
# 引用变量
echo "using $file_name to configure something"
# 将命令的结果赋值给变量
config_files  = $(ls config)


# conditionals
# 查看config是不是一个目录
if [ -d "config" ] # 注意: 这里的两个方括号前必须要有空格
then
	echo "reading config directory contents"
	config_files = $(ls config)
elif [ -f "config" ]
	echo "do something"
else
	echo "config dir not found. Creating one"
	mkdir config
fi


# operation
num = 10
if [ num -lt 10 ] # 小于10
-gt # 大于
-ge # >=
-ne # not equal

# 事实上, condition也可以写 [[]], 这是bash的写法, posix不支持



# compare two string
user_group = $1 # read input from command line
if [ $user_group == "xiao" ]

read -p  "Please enter your password: " user_pwd # save the user input to user_pwd
echo "thanks for your password $user_pwd"

# 在执行命令的时候, 如果没有控制输入变量的个数. 即使这些变量没有在脚本中被引用, 但是也是可以通过下面的方法来取得信息
$* # 代表所有的的用户输入参数用一个字符串表示
$# # 一共提供了多少个参数
$? # 获取到上一条命令的返回值

# for loop
for param in $*
  do
    echo $param
  done

# while loop
sum=0
while true
do 
  read -p "enter a score" score
  if [ $score == "q" ]
  then
    break
  fi
  sum=(($sum+$score))
done
echo "total score: $sum"

$sum + $score # 注意: 会被解释为两个字符串的相加
$(($sum+$score)) # 才会被解释称两个数字的运算
```
在bash脚本中写函数

```shell
function sum() {
 total = $($1 + $2)
 return $total
}

sum 2 10 # 这里直接调用了函数, 并传入两个参数
result=$? # 这里获取到了调用函数得到的结果

echo "The result is $result"
```

设置环境变量
```shell
# 临时设置环境变量
export DB_USERNAME=dbuser
# remove the environment variable
unset DB_NAME

# 永久设置环境变量
# 修改 ~/.bashrc 增加
export DB_USERNAME=dbuser
source ~/.bashrc # 使修改生效

# PATH 环境变量
# 如果只是给某个用户使用, 在到该用户的 ~/.bashrc下修改
PATH=$PATH:/NEWPATH
```

模式
- vim 有下面 4种模式

| 模式      | 作用         | 转换                              | 注释                    |
| ------- | ---------- | ------------------------------- | --------------------- |
| Normal  | 移动光标       | `i` 转 Insert 模式                 |                       |
| Insert  | 键盘输入       | `jj` 或者 `esc`退出 Insert 到 Normal |                       |
| Vistual | 选择内容       | `v` 进入或者退出 Visual 模式            | ctrl + v block select |
| Command | 查找, 替换, 保存 | 在 normal 模式下, 输入冒号 :            |                       |
### vim 配置
```shell
# 修改配置文件 .vimrc
set ts=4 # 一个 tab 是 4 个空格
set expandtab # 用空格代替 tab
set ai # 自动缩进
set paste # 
```

### 模式转换
在普通模式下移动光标, 并插入
```shell
i # 在光标前插入 
a # 在光标后插入 
I # 在行首插入
A # 在行尾插入 

o # 在当前光标的下一行插入
O # 在当前光标的上一行插入
```
在命令模式下移动光标, 但是不插入
```shell
$ # 将光标移动到行位
0 # 将光标移动到行首
```
光标跳到某一行
```shell
gg # 文档头部 
G # 文档底部 
12G # 跳转到第 12 行 
```
替换
```Shell
%s/old/new 
```
撤销
```shell
u # 撤销 1 个动作
10u # 撤销 10 个动作
N # 往前走
```

# VIM 命令模式
保存和退出
```shell
shift + ZZ # 存盘退出
shift + ZQ # 不存盘退出

# 文件打开直接到指定行
vim +11 test.sh
```
光标行间移动
```shell
H # 当前页的第一行
M # 当前页的中间
L # 当前页的底部

w # 下一个单词的词首
e # 当前或者下一个单词的词尾
b # 当前或者下一个单词的词首
5w # 下面 5 个单词
```
光标行内移动
```shell
h # 左移
l # 右移
j # 下移
k # 上移
^ # 非空白字符的行首
0 # 包含空白字符的行首
$ # 行尾
```
段落移动
```shell
} # 下一段
{ # 上一段
```
复制行
```shell
yy # 复制光标当前行
3yy # 复制光标当前所在的连续三行

100iwang+esc # 把wang 复制了 100 次
```
删除替换
```shell
# 删除字符
x # 剪切一个字符
r # 替换一个字符
R # 进入 replace 模式

# 删除单词
diw # 删除一个单词, 光标在单词的任意位置
dw # 删除光标所在位置到单词结尾

# 删除行
dd # 删除当前行
3dd # 删除当前行一下3行
d0 # 删除到行首
d$ # 删除到行末
dgg # 删除到文件开头
dG # 删除到文件末尾
```
大小写
```shell
~ # 大小写切换
```
快速删除换行符
```shell
J # 不需要把光标切换到行末, 直接把下面一行提上来
```
撤销命令
```shell

ctrl + r # 重新执行撤销的命令
. # 重复执行上一个操作
. # 重复执行上一个操作 9 次

```
搜索
```shell
/ # 搜索 n 下一个 N 上一个
```
高级用法
```shell
# 删除双引号中间的内容
di"
# 删除中括号中间的内容
di[ 
```

# 网络

```shell
# 两条非常有用的网络命令
# 注意: 使用这两个命令都需要安装 net-tools
apt install net-tools
ifconfig
netstat -lpnt # 查看 active connection, 查看端口的占用

ps aux | grep java  # 查看应用占用的资源, 进程ID
nslookup # 查看对应网址的域名解析
ping
```

SSH
1. ssh 可以用来传文件
2. ssh 可以用来cmd line 访问
ssh key pair = Private Key + Public Key

ssh 使用22端口, 所以被访问的服务器, 需要在防火墙上放开22号端口. 但是防火墙上也需要控制来源的IP地址, 整个也称为限源. 即给来访问的某些IP开白名单

使用密码访问
```shell
# 使用ssh 访问, 密码方式
ssh username@ip_address
# 需要输入密码
```

使用秘钥访问
```shell
# 使用ssh key pair 访问
# 生成密码对
ssh-keygen -t rsa
# 此时生成 id_rsa 私钥, 和 id_rsa.pub 公钥

# 到remote服务器上, 去把客户端的公钥添加进去
vim .ssh/authorized_keys
# 此时再从客户端访问服务器
ssh root@ip_address # 不再需要输入密码


# 注意如果客户端有多个私钥, 则需要指明使用那个私钥
ssh -i .ssh/id_rsa root@ip_address
```

复制文件到远端服务器
```shell
scp test.sh root@11.0.1.152:/home/xiao
# if the port is changed
scp -p 223 test.sh root@11.10.1.151:/home/xiao
# all file and folders in current dir
scp -r . ubuntu@12.2.2.2:/home/ubuntu/app 

```

# git
分布式的版本控制工具
version control
- working directory 
- staged area 
- local-repository
- remote repository

```shell

git log # show commit history
git log --oneline # show oneline commit
```

把本地的项目推送到远端
```shell
# create local git repository
git init 
# then you need to create remote repo on gitlab or github
# then you add remote repo as origin with address
git remote add origin git@address
git push
```

克隆远端的项目到本地
```shell
# clone remote repository
git clone ssh@address
```
branch
- 一般情况下, 只要有一个master branch 和 develop branch. 
- 在develop branch 下还有feature 和 bugfix branch. 这两个branch向 develop branch merge
- 在每个sprint后面, develop branch向master 合并
- 注意, 我觉得一般不会在本地将其他branch merge到main branch上, 而是在分支的feature branch push到remote后, 在remote 申请 pull request, 经过主管同意后, 在merge到main上
- 实际工作中, 往往是开发者在自己的branch上工作了很久, 其他的开发者对master branch进行push, 并被接受了, 所以master branch上的内容要更新. 开发者先要pull 最新的内容到本地master branch上, 然后, 在checkout 到feature branch上, 做merge main的操作. 当本地开发完成之后, push feature branch到remote, 再申请pull request, 合并到master branch上
- 通常情况下, 当一个branch本合并后, 是需要删除的. 如果有新的问题, 再创建新的branch
- 注意remote的branch如果被删除, 本地的branch应该还在的. 如果此时git pull, 会得到提示 but no such ref was fetched. 此时进入main branch, 运行git pull 获得merge之后的最新代码. 运行 `git branch -d <branch_name>` 删除分支branch
```shell
# 首先产看是否remote 仓库有新的branch
git pull
git branch # show all branches
git checkout <branch_name>

git checkout -b <new_branch_name> # 创建一个新的branch
git add .
git commit 
# 如果remote没有本地新建的branch, 则需要使用下面的命令, 先创建branch再push变更
git push --set-upstream feature/database-connection 

```

Trunk Based vs Feature Based
`git pull --rebase`
如果两个程序员在一个branch上修改. 程序员B修改, 并提交了一个commit, 程序员A不知道. 此时他把他自己修改的部分提交了, 就会遇到报错. 让后他想用 git pull 把程序员B修改的代码拉下来, 但是也会冒出一个 merge conflict 需要处理. 因为git pull = git fetch + git merge 就是已经将远端的新代码和本地做了合并. 合并不起来就有conflict, 就需要手动处理. 当你修改完本地conflict后, 你可以用 git push 将代码推送到远端. 问题是, 整个push里面会包含两个commit, 一个commit是程序员B做的, 一个commit是程序员A自己的做的. 因为程序员B做的commit, 在他自己的commit中已经提交过了, 所以这里再提交一般就重复了. 所以会使用 git pull --rebase 就是把开始拉去程序员的改动到本地时, 就把本地的git 状态修改为同步之后. 这样就只要考虑自己的变更

```shell
git pull -r # 就是rebase
# 如果遇到conflit, 就修改conflict 文件
# 修改conflict文件后, 运行下面的代码继续做 rebase
git rebase --continue
# 上面rebase完成后, 在push本地的代码到remote
git push 
# 成功后, 远程就看到一个新的commit, 这里我本地修改好的. 
# 且我本地推送上去的, 不会出现两个commit
# 所以在明确知道一个branch上有多个同事在修改时, 一定要使用 git pull -r 来经常rebase自己的代码
```

gitignore
```shell
/folder/* # ignore folder
.DS_Store # ignore file
**/.DS_Store # under any folder
# untrack the tracked the folder, 这些文件可以是已经commit过并push了的
git rm -r --cached .idea # 被remove 的文件会出现在 git status 中, delete 的状态
```
git stash
use case
1. 如果你有没有修改, 但是有没有达到可以提交commit的地步. 但是此时你有需要去其他的branch去修改代码. 此时如果你直接checkout master, 就会报错. 提示需要commit或者stash当前代码
2. 如果你修改的代码, break了, 你想退回最初的代码, 此时上一次commit在修改之前是否能正常运行
```shell
git stash # 将改动的代码都放到cache中, 此时在 git status 的commit信息中, 就看不到修改了
# 所以当你切换到新的分支时, 先要看看有没有没有处理的stash内容
git stash pop # get the changes back 
```
go back to previous version
```shell
# 如果要回退到之前的某个版本
git log # 先查看要回去的commit id
# 注意, 需要先确保当前分支没有没有报错的commit, 才能切换. 否则用 stash 先暂时保存一下
git checkout <commit-id>

# 此时会提示, 我在 detached HEAD state, 表示我不在最新most-updated的版本上. 
# 注意: 你不能在这个分支修改, 如果要做实验, 或者修改测试, 需要先在整个commit上新建一个分支. 
# 回到 most-updated 版本上
git checkout branchname

```
undo commit
如果还没有push到remote
```shell
# 注意, 整个操作会直接修改本地的git log, 上一次修改的所有内容都会消失
git reset --hard HEAD~1 # 撤销上一个commit, 如果是撤销前两个commit, HEAD~2 

# 如果只是修改上一次commit中的部分代码, 保留大部分的代码不变. 
# 测试在 git log 中上次commit已经删除. 但是被修改的文件保留在working directory中
git reset --soft HEAD~1
```

如果只是修改上一次commit, 不删除 git log 中信息
```shell
# 如果你已经commit, 然后发现有问题, 修改后, 想把代码并入上一次的commit中
# 直接先修改代码, 然后
git commit --amend -m "modifed last commit message"
```
如果你的代码已经push到remote
注意: 下面的操作绝对不能在master或者develop的branch上做, 因为此时有可能有别的开发者已经下载你之前删除的代码, 当他们再次提交自己的修改时, 会发现之前rebase的内容已经被你删除了. 造成reference问题. 所以只能在只有自己的branch上做
```shell
git reset --hard HEAD~1 # 现在本地删除这个commit
git push --force # 将本地的变动强制推送到remote. 注意如果远端branch是protect, 就不能被这种方式修改
```
如果你要删除已经在master和develop branch上的commit, 只能用revert
```shell
git revert <commit-id> # 如果你需要删除上一次的变化, 就要放最后一次commit的ID
git push # create a new commit to rever the old commit 
```

# Build & Package Manager Tools
封装: 就是把你在开发环境里用到的包, 和你的源代码一起封装打包, 然后这样就可以放到服务器上去直接部署了. 而不需要从头到尾去安装和下载依赖
Build code 
- Compiling
- compress
代码的版本也需要分为 dev, test, production
artifact repository
Artificat: including whole code plus dependencies
java - JAR, WARn(把前端的react包和后端的jar包, 打包在一起的一种技术)

Maven可以正常运行需要满足三个条件
1. Java SDK
2. Java in "Path"
3. Set JAVA_HOME

```shell
mvn --version
mvn package # build our project
mvn install # build our project in target folder
```

Gradle 安装
1. 下载并解压, 添加到环境变量
2. 配置Intellij
```shell
grade build # in build folder
```
使用java命令, 运行jar包
```shell
java -jar <file.jar> & # 最后 & 表示关掉terminal 也可以运行
# 查看正在运行的 java 程序
ps aux | grep java # 看到进程号 2804
root        2804 36.3 26.7 2227164 125548 pts/0  Sl   04:20   0:08 java -jar java-react-example.jar
root        2835  0.0  0.4   7076  2048 pts/0    S+   04:20   0:00 grep --color=auto java

# 使用 netstat -lpnt 命令查看java程序监听的端口是 7071
tcp6       0      0 :::7071                 :::*                    LISTEN      2804/java

# 需要确认防火墙上已经开启了7071端口

```

nodejs
webpack
- transpiled
- compressed/minified
- Build Tool / Bundler 

```shell
# 在有 package.json 的目录下
npm install
# 进入app的目录下, 有server.js 的地方, 运行
node server
# 或者
npm start
npm stop
npm test
npm publish 

# pakage manger npm, yarn 不是封装工具, 没有build
# package manager just install dependency
# 在部署的时候, 复制artifact 和 package.json到目录下
# 安装 dependency
# unpack zip/tar
# run app
npm pack # 给程序打包 .tgz

```
# digital ocean
在digital ocean上的ec2被称为 droplet, 
可以在setting 中统一配置ssh pub-key, 这个key可以用于所有的droplet的访问
在networking中可以配置防火墙, 限制ssh访问的IP地址, 然后关联到droplet上去
在ubuntu系统中安装java 17

# Artifact repository Management

**Sonatype Nexus Repository** 是一种artifact repository manager 产品
市场上还有public artifact repository manager, 比如 Maven Central Repository, NPM
Artifacts: Apps built into a single file
Repository Type
- proxy: 我们从maven-central把会用到的依赖下载到Nexus作为缓存. 以后相同的包就不用从Maven-Central中下载了. 可以直接从Nexus 中找. 这种模式需要在本地IDE工具中配置Maven的源
- host: 我们本地自己开发的代码, 可以推送到Nexus的repository中
- group: 如果一个repository即充当了proxy和host的角色, 我们就称这种类型是 group
Component 和 Asset的区别
- A component can have one or more assets
- Each asset belongs to a component.
- Use **components** when you think about _logical versions_ of software.
- Use **assets** when you think about _actual files_ stored on disk.

Nexus 在整个CICD Pipeline中的位置
![[Pasted image 20250826072721.png]]

Features of Repository Manager
- Integrate with LDAP
- Flexible and powerful REST API for integration with other tools
- Backup and restore
- Multi-format support (different file types - zip, tar, docker etc)
- Metadata tagging (labelling and tagging artifacts)
- Cleanup policies
- Search functionality

# Docker
docker和virtual machine的区别呢, docker virtualize 应用层. 但是Linux core是使用宿主机的. Virtual Machine是linux core和应用层一起虚拟出来的
- size: docker 小
- speed: docker 快
- compatibility: 在linux上变异的docker container就以为是使用linux内核, 所以只能运行在linux的服务器上. 在windows上开发就需要wsl了

docker engine
- server
- api
- cli
docker enginer components
- Container runtime
	- Pulling images
	- Managing container lifecycle
- Volumes
	- Persist data
- Network
	- Configuring network for container communication
- Build Images
	- Build own docker image
Docker image
- Layers of image, 如果某个layer本地有了, 则不需要下载. 比如不同版本的postgres, 在很多层可能是一样的, 比如base image, 就不用下载
- 大部分 Linux base Image 是 alpine:3.17 因为小

docker image 相关的命令
```shell
# pull images
docker pull redis 
# show local existing images
docker images

```
docker container 相关的命令
```shell
# 构建container中的环境变量
# postgres:13.10 是image的版本,如果本地没有就从docker hub上拉取
docker run -e POSTGRES_PASSWORD=myscretpassword postgres:13.10 
docker run -d # detach mode, 在后台运行, 返回container id
# 查看正在运行的container
docker run -p <host_port>:<container_port> -d redis
docker ps 
docker ps -a # including not running or stopped container

docker stop <container_id>
docker start <container_id>
```

# AWS
- 创建一个账户后, 会自带一个vpc 对应你选在的region, 每个vpc会自带3个subnet, 对应三个AZ
- 

## AWS CLI
```shell
# install on mac
brew intall aws
aws --version 

# config aws account
aws configure
# get current cli user name
aws sts get-caller-identity

```


aws cli 安装完成后, 会在家目录下产生一个目录 `~/.aws`, `~/.aws/config`文件定义了默认的region和输出格式(一般选择json). 同事默认的用户的ak/sk 保存在 `~/.aws/credential` 文件中

创建 ec2 instance 需要先创建 Security Group 和 key pair

```shell
# 创建 ec2 instance
aws ec2 run-instances \
--image-id ami-0360c520857e3138f # 从aws页面上获得
--count 1
--instance-type t2.median
--key-name m4macbookair
--security-group-ids sg-08e3efc606ea1b353
--subnet-id subnet-0154f9bf85b890da1

# 获取 ec2 instance
aws ec2 describe-instance
```
获取到subnet id
```shell
aws ec2 describe-subnets
```

创建 Security Group
```shell
# create security-group 创建sg必须是要与vpc绑定的
aws ec2 create-security-groups --group-name my-sg --description "My SG" --vpc-id <vpc-id>

# show security groups
aws ec2 describe-security-groups --group-ids <sg_id>

aws ec2 authorize-security-group-ingress \
--group-id <group-id> \
--protocol TCP \
--port 22 \
--cidr 95.39.129.114/32
```

创建 key pair
```shell
aws ec2 create-key-pair \
--key-name MyKpCli \
--query 'KeyMaterial' \ # 如果没有这个参数在回返回 KeyName 等其他, 字段
--output text > MyKpCli.pem # 把返回的字段输出到文本文件中. 在mac或者ubuntu上使用.pem格式
```
使用ssh访问ec2
```shell
# 修改permission
chmod 400 MyKpCli.pem # 必须要去除others的访问权限
ssh -i MyKpCli.pem ec2-user@3.67.39.67

```
注意: 不同的镜像使用的用户名是不同的
```shell
Amazon Linux / RHEL → use ec2-user
Ubuntu → use ubuntu 
Debian → use admin
CentOS → use centos
```


# K8s

## k8s的组件
Pod k8s 的最小单位. 在一个 pod 上含有一个或者多个 container.
  - 一般在一个 pod 上只部署单个应用. 因为多个容器可以共享同一个 pod 中的一片存储
  - 在一个 pod 内部, 多个 container 之间是 shared network namespace. 对外面向别的pod时用同一个 IP 地址, 使用相同的端口范围.
  - 一般在一个 pod 内部, 放一个 main app container 和它的 sidecar container. Sidecar 通过 localhost 和 main app 交互
  - 默认情况下每次 pod 被重构都会被分配一个新的 IP 地址. 而 pod 中的 container 是没有独立 IP 地址的

Service 是 K8s 构建的内部虚拟网络. 用户不同的的pod之间相互通信. 
- Service是独立于Pod存在的. 即如果pod被销毁了. Service不会一起被销毁. 它会找到备份的pod关联上. 
- Service给每个 Pod 分配一个 Permanent IP, 注意不是给每个 container 分配一个 IP 地址.
- Service 是pod之间内部使用. 如果K8s要接受外部的流量请求, 则是通过另一个组件Ingress来处理
- Service和Ingress的另一个区别在于, Service的双方通信地址是 Pod的IP地址, 而Ingress提供给外部访问的地址是 node的ip地址和port号, 再有port号映射到Service的分配给Pod的IP地址上. 整个思路我理解有点像NAT
- 如果一个 pod 在另外一个 node 上有一个备份 pod, 主备 pod 可以共用一个 service, 实现 HA, 或者作为负载分担.

Ingress 是面向公网访问请求提供的组件. 为公网访问提供 URL, HTTPS验证, 并将外部流量通过负载分担的方式分配给 Service

Deployment 是blueprint. 定义了需要pod需要多少个replica. 
- Deployment 只能处理 Stateless的应用, 不能用来处理数据库这种stateful的应用. 因为数据库是Stateful, 在多个replica的情况下会有consistency 一致性的问题. 就是写入的数据的时候, 从那个replica先写入. 另外一个replica就有数据不一致和同步问题. 在K8s中提供了处理stateful 应用的blueprint 工具, Stateful Set

StatefulSet (STS) 是用来处理stateful 应用, 比如数据库应用的blueprint. 但是通常的做法是数据放在K8s集群外部, 不通过K8s进行管理

Configmap 用来配置pod 的. 因为有些pod, 比如环境变量这些信息, 应该放在image外部配置, 这样环境变量的信息稍有改动, 就不需要重新build整个镜像. 比如数据库的URL. 对应的另外一个概念是Secret

Secret 用于保存哪些不同被公开的配置信息, 比如访问数据库的用户名和密码

Daemonset 在node scale down和scale up时, 简化了在 Deployment 中手动修改 replica 数量的问题. 确保每个node上只有一套pod replica, 即pod被均匀分布在不同的node上
在 Node增加时, Pods会自动增加到node上, 在node减少是, Pod会被garbage回收

Volume 用于数据持久化. 数据可以保存在本地的 pod 所在的 node 上, 或者在远端非 K8s Cluster 上

namespace 不同的namespace 不能分享的资源
- configmap 和 secret不能分享
可以分享的内容
- service
不在任何namespace中的, 而是只属于cluster的
- volume是不在任何namespace中的
```shell
kubectl api-resources --namespaced=false # 查看不在namespace中的资源
kubectl get ns 
kubectl get pod -n kube-system
```
k8s本身是不带有网络组建的. 网络组建的目的是使得个pod之间能够通信. 实现网络组建的接口定义是k8s完成的. 有几个标准
- 没有的node在一个子网中. 但是网段和pod所在的网段是不重叠的. 相当于从pod到node是经过一次NAT
- 每个node上的所有的pod是在同一个子网段里. 从一个node上的pod向另外一个node上的pod进行通信是, 利用路由表进行通信

## K8s的进程 Process
在worker node上的进程必须有 3 个. 可选一个Kube log collection
1. Container Runtime 并非docker image, 而是通过image构造出来的实例. 
2. Kubelet Kubelet可以看做是worker node和在node上的pod的接口. master node发送配置需求给某个node上的kubelet, kubelet查看当前node上的资源, 具体去创建和销毁node上的pod
3. Kube-Proxy 可以看做是Service和Pod之间的接口. 因为Service和Pod其实是分离的, Service其实是通过Kube-proxy来找到pod. 但是Kube-proxy也是有智能的, 他会把流量优先分配给和发出请求的pod在同一个node的上到目标pod replica, 而不是别的node上的pod replica

在Master node 也称为control plan 上的进程,
1. API Server 在K8s中, 所有的对pod或者node的操作, 都是一次API调用, 都通过API Server来实现. 同时作为gatekeeper 和 authentication 不是所有请求都是安全可靠的. Kubectl 就是和ApiServer打交道
2. Kube-scheduler 决定了一个新的pod要在那个Node上创建, 取决于当前node 的cpu和内存资源占用情况. 然后通知Node上的kubelet进行具体的pod操作
3. Controller Manager 首先是监控所有的node的状态. 如果一个node down了, 在整个node上的pod就都挂了. 根据deployment中node replica的数量, 我们就需要再别的node上去创建pod. 基于已经定义好的manifest, 如果发现某个node有问题, 就通过kube-scheduler 决策调整方案, 然后通知node 上kubelet去实施
4. ETCD 是一个Key-Value数据库, 所有的对于node和pod的操作历史都保存在这个数据库中

### TLS

## Minikube
Minikube 需要运行在docker内, 在minikube容器内部也有一个docker来运行类似k8s的实例. 一个实例

```shell
# 安装minikube
brew install minikube
# 启动之前必须要先启动docker
# 启动minikube
minikube start --driver=docker --memory=4096 --cpus=2
# 查看 minikube 运行状态
minikube status
# 默认情况下, minikube 已经安装了kubectl
kubectl get nodes

```

if you have issue to start minikube
```shell
# 如果遇到minikube起不来, 可以先删除, 然后重新建立. 并指定容器所需要cpu数量和内存
minikube delete
minikube start --driver=docker --memory=4096 --cpus=2
```

kubectl commands 以下这些命令主要是排错时用
```shell
kubectl create deployment nginx-depl --image=nginx 
kubectl get deployment
kubectl get deployment -l app=nginx
kubectl get pod
kubectl get replicaset # 整个命令可以看到之前被销毁的pod

# 修改 deployment. 每次修改deployment都会删除现在的pod, 重新构建pod
kubectl edit deployment nginx-depl 
# 查看pod内部的log
kubectl logs <pod_name>
# 
kubectl describe pod <pod_name>
# 进度container 
kubectl exec -it <pod_name> -- bin/bash

kubectl delete deployment <deployment_name>
```


```shell
kubectl apply -f <yaml_file>
```
deployment yaml file
```yaml
apiVersion: apps/v1
kind:Deployment
metadata:
  name: ngnix-deployment # deployment name
  labels: 
	app: nginx # 这里是删选deployment时用的label
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx # 这里是告诉deployment 去找到 label是 app: nginx 的pod去处理
  template:
    metadata:
      labels: 
        app: nginx # 这里定义了 pod使用app: nginx这个label, 和上面的 selector中的label必须一致
    spec:
      containers: # 这里是container的要求
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```


## 安装K8s 1.28 on Ubuntu 24.04

1. 关闭swap
```shell
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl


# 所有3个server都需要
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
# 配置master和node不同的端口号, 在aws上用security group来配置
# 配置hostname
cat <<EOF | sudo tee -a /etc/hosts 
172.31.1.92 master
172.31.1.152 worker01
172.31.1.45 worker02
EOF

sudo hostnamectl set-hostname master
# 重新连接 ssh

```

安装containerd
创建一个 install.sh
```shell

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF


sudo sysctl --system

sudo apt install -y containerd
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd

```

安装kubeadm, install_kubernetes_components.sh
```shell
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update


sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

apt-cache madison kubeadm

sudo kubeadm init --kubernetes-version=1.28.0
# 安装完毕后
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl get nodes
```


```shell
kubectl cluster-info
kubectl create namespace my-namespace

kubectl get namespace
kubectl get pod -n kube-system

```

join control panel
```shell
kubeadm token create --print-join-command
sudo kubeadm join 172.31.1.92:6443 --token k5xkj8.6ob3olyij6mdtlxc --discovery-token-ca-cert-hash sha256:5697d662e32341348b76f9058aa150b36eb7c413207d153ed3ec7ce455fca32b

# 需要打来6783 端口
# 查看日志
kubectl logs weave-net-znr4h -n kube-system -c weave
# 查看3个node上守护进程的状态
kubectl get pod -n kube-system -o wide | grep weave

kubectl exec -n kube-system weave-net-znr4h -c weave -- /home/weave/weave --local status

weave-net-b2skb
weave-net-czxjg
weave-net-znr4h
```


### Service
```yaml
apiVersion: v1
kind: Service
metadata: 
  name: my-service
spec:
  selector: 
    app: MyApp # 这里制定了给某个deployment用
  ports:
  - protocol: TCP # 这里默认是clusterIP, 在在创建的时候才有的
    port: 80 # 暴露给外部的端口号
    targetPort: 9376 # development 容器内部端口号
```

```yaml
kubectl describe <component> <component name>

# 查看 label
kubectl get pod --show-labels
# 过滤出 label
kubectl get pod -l app=nginx
# show los of all pods with same label
kubectl log -l app=nginx
```

scale
```shell
kubectl scale deployment nginx-deployment --replicas=4
```
record the history
```shell
# record the history of change
kubectl scale deployment nginx-deployment --replicas=4 --record 
# view the history of chagne
kubectl rollout history deployment nginx-deployment
```

kubectl run 用于测试
- 使用curl命令测试 pod是否正常运行
```shell
kubect get svc # show name and ip address of the service
kubectl run test-nginx-svc --image=nginx
kubectl get pod 
# enter into the pod container interactive terminal
kubectl exec -it test-nginx-svc -- bash

# now inside of the container
curl http://10.96.0.1:8080

```
coreDNS
需要给pod中的container 配置 dns, 从 k8s 1.2之后. 需改 `/etc/resolv.conf`, 但是kubelet 在创建pod的时候, 已经主动配置了. 通过查看 `sudo cat /var/lib/kubelet/config.yaml`
Troubleshooting: 如果发现用service name无法访问, 但是ip地址可以访问. 那多半是两个pod不在一个namespace里
```shell
# search for the ip address of dns service
kubectl get svc -n kube-system | dns

kubectl exec -it test-nginx-svc -- bash
vim /etc/resolv.conf
# edit it as
nameserver 10.96.0.10
```
coreDns 给service 配置的域名规律是
```shell
# 如果是同一个namespace
<servicename> 开始可以访问的
# FQDN Fully Qualified domain name
# 只在在访问不同namespace下的service的时候, 才需要FQDN
<servicename>.<namespace>.svc.cluster.local
<ervicename>.<namespace> #可以简写, 是因为在 /etc/resolv.conf 中有search的entry, 已经在域名中搜索了

```

k8s 重要的服务的配置文件都是放在 `/etc/kubernetes/manifests/`
service 的cluster ip CIDR 是在 `/etc/kubernetes/manifests/kube-apiserver.yaml` 中定义的 `--service-cidr` 这个文件本身是 kube-adm 创建的. 有一个默认值. 
```shell
kubeadm config print init-default # review config

```
任何一次修改了 kube-apiserver.yaml 文件, kubelet 会周期性的发现变动, update
新的cidr只对新建的service起作用, 不会修改老的service
```shell
kubectl create service clusterip test-new-cidr --tcp=80:80
kubectl get svc

```

preview or dry run 这样可以快速创建template
```shell
kubectl create service clusterip test-new-cidr 80:80 --dry-run=client -o yaml>my-svc.yaml

kubectl create deployment my-deployment --image=nginx:1.20 --port=80 --replicas=3 --dry-run=client -o yaml > my-deployment.yaml

kubectl run my-pod --image=nginx:1.20 --labels="app=nginx,env=prod" --dry-run=client -o yaml > my-pod.yaml
```


signing certificate
一个用户要能方位k8s cluster, 需要申请certificate, 这个certificate需要被k8s的CA签名, 步骤如下
1. 使用openssh 创建一个私钥
2. 将私钥的内容放入 certificate signing request 中, 并构建一个csr对象
3. k8s 管理员approve 这个csr
4. 然后把这个approved的csr 保存为, 公钥证书 crt
```shell
openssh genrsa -out dev-tom.key 2048
# create certificate signing request fro the key
openssh req -new dev-tom.key -subj "/CN=tom" -out dev-tom.csr 

```

编辑一个yaml文件 `vim dev-tom-csr.yaml`
```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: my-user-csr   # <-- change this to something unique
spec:
  groups:
  - system:authenticated   # optional, depends on RBAC
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBS...   # <-- base64-encoded CSR
  signerName: kubernetes.io/kube-apiserver-client   # common for client certs
  expirationSeconds: 31536000   # optional (1 year), default is 90 days
  usages:
  - client auth
```

```shell
# create certificate signing request
kubectl apply -f dev-tom-csr.yaml 

# list all the signning reqeust, which is in pending status
kubectl get csr 
# find current k8s user is kubernetes-admin
cat ~/.kube/config | grep kubenetes-admin -A

# approve the certificate
kubectl certificate approve dev-tom
kubectl get csr dev-tom -o yaml

# encode the certificate
base64 dev-tom.crt | tr -d "\n"

# which you can think a public key to sign in k8s
echo 'base64 encoded csr' | base64 --decode > dev-tom.crt
```

```shell
kubectl option
# find control plan api-service ip address
kubectl cluster-info 

# tell kubectl to use dev-tom to authenticate the k8s access
# 注意这里 ~/.kube/config 中不能有文件
kubectl --server https://172.31.44.88:6443 \
--certificate-authority /etc/kubernetes/pki/ca.crt \
--client-certificate dev-tom.crt \
--client-key dev-tom.key 
get pod

# 修改config文件, 创建profile, 创建certificate和key的文件路径

kubectl --kubeconfig dev-tom.conf get pod
# 或者把 dev-tom.conf 直接复制到 ~/.kube/config 下, 就可以直接用kubectl来访问了

```


```shell
kubectl create clusterrole dev-cr --verb=get,list,create,update,delete --resource=deployments.app,pods --dry-run=client -o yaml > dev-cr.yaml
# 默认有两个apiGroups, 一个core group, 一个其他group

kubectl api-resources

# binding the cluster role to user
kubectl create clusterrolebinding dev-crb --clusterrole=dev-cr --user=tom --dry-run=client -o yaml > dev-crb.yaml

kubectl describe clusterrolebinding dev-crb

# 查看tom是否有权限做 create pod
kubectl auth --kubeconfig config can-i create pod --as tom 
```

service account
```shell
kubectl create serviceaccount jenkins --dry-run=client -o yaml > jenkins-sa.yaml

```