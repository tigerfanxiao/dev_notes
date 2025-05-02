
#### 在指定文件夹进入 cmd
- 在文件夹的路径窗口里面敲 cmd, 可以直接在该目录下打开 cmd 
#### 使用管理员权限打开 CMD
- `win + R` 进入命令提示窗口. 输入 cmd, 使用 `ctrl + shift + enter` to launch CMD
### 常见命令
-   `d:` 直接进入D盘. 首先确认你电脑上确实有 d 盘哈
-   `dir` 列出当前目录下的文件以及文件夹, 作用同linux中`ls`
-   `md` 创建目录, 等同于linux下的`mkdir`
-   `rd` 删除目录(文件夹) 必须保证文件夹是空的
-   `del *.txt `删除目录下所有txt文件
-   `echo fanxiao > ee.txt` 将fanxiao写入创建的ee.txt文件
-   `type ee.txt` 查看ee.txt文件内容
-   `cd directory`进入当前目录下的directory文件夹
-   `cd..` 回到上一个目录
-   `cd\`  退回到根目录
-   `exit` 退出DOS命令行
-   `start` 新开cmd窗口，但沿用之前窗口的环境变量
-   `cls` 清屏
```shell
# 查看所有用户
net user
# 类似于linux 的/home
c:/system/Users/Administrator
```


# 查看序列号
查看序列号
```shell
wmic bios get serialnumber
```
### cmd 中文乱码

# DNS
打开文件
`c:\windows\System32\drivers\etc\hosts.file`
修改里面的ip地址和域名


# Wifi 密码查看
本机的在用的SSID 密码

![[Pasted image 20230926102430.png]]

# SSH
## 在windows 上生成sshkey
```shell 
# github 使用
ssh-keygen -t rsa -C "your_email@youremail.com"
```

### 在windows中连接ssh
```shell
ssh root@hotname.com
ssh root@172.13.12.1
ssh -p 9765 root@172.18.31.1 # 如果对端的IP地址已经做了修改
```

# 问题
1. win10上本地和git bash中使用ssh agent并不是一个。 gitbash中有独立的ssh。在gitbash中使用git push调用的是gitbash中的ssh agent， 感觉理论上需要配置ssh-add 列表和打开gitbash就自动启动


在win10上安装ssh服务端
setting - Optional Feature - Add a Feature - OpenSSH

在文件浏览器的地址栏中输入
%programdata%
打开ssh folder

启动ssh服务
computer management - Services and Application - Services 
在右面的列表中找到
OpenSSH SSH Server 双击打开属性
service name sshd
change Startup type automatic 开机启动
点击start
服务启动后，在ssh folder下会生成很多启动配置文件

查找到本机的ip地址

