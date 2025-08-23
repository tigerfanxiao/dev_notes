Ubuntu 22.04
```shell
# check Ubuntu version
lsb_release -a

uname -a
cat /etc/os-release
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

# 用户管理
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
if [ -d "config" ]
then
	echo "reading config directory contents"
	config_files = $(ls config)
else
	echo "config dir not found. Creating one"
	mkdir config
fi

```



VIM
```shell
# vim 练习模拟换机
vimtutor 

vim +10 test.txt # 直接进入第 10 行
```
# 模式
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
I # 在行首插入, 或者 0
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

