
我们现在是弯道超车，3个月学习别人三年的知识，有几个要求
1.少用之前这儿听得那儿学的，与老师讲的博弈，没有时间
2.老师讲什么咱们就做什么，至于老师对不对，现在不要去想，等你完全能把老师讲做出来，后面再去想这个，你就明白了
3.之前你这儿看一个文章，那儿收藏一个视频都不是学习，甚至可能是错误的，现在要空杯状态，老师讲什么咱们就学什么，遇到问题就在群里发出来。

咱课程的一些软件呀 镜像呀 因为有的开源的网站都可以下载 所以咱们课程都不没提供，
我给你们找了一些我带的其他班学生整理的一个链接，是咱们课程中能用到的一些软件镜像 你们可以收藏一下哈
vm，markdown编辑器，系统镜像等链接: https://pan.baidu.com/s/1XbaviYkwDVQ4B5A1WuYNFg?pwd=p8v8 提取码: p8v8 
ubuntu镜像网络下载地址：https://mirrors.ustc.edu.cn/ubuntu-releases/24.04/ （按照版本修改对应下载即可 24.04->22.04）
ubuntu镜像历史版本下载地址：https://mirrors.ustc.edu.cn/ubuntu-old-releases/releases/
rocky8网络下载地址：https://dl.rockylinux.org/vault/rocky/8.6/isos/x86_64/ （按照版本修改对应下载即可 8.6->8.9）
xshell免费版安装教程：https://blog.csdn.net/m0_52985087/article/details/135643689
tips:md文件使用markdown编辑器下载，思维导图需要使用xmind打开


咱班课程为录播，是是实时更新北京面授班线下班的课程，面授班的上课时间是135（也有可能是246），9-18点；246是练习的时间，12.27号开班
所以每周的246会给咱班同步过来视频和资料，在网盘链接里面（基本两天同步一次）

24年就业课程链接：https://edu.magedu.com/course/vip/1804

给你总结下你报名后拥有的视频
1.赠送24年全套视频（咱的课都是分为就业和架构两部分；就业课程在网页版平台观看，架构是授权观看）
2.咱班N100课程，每周135上完后课，第二天会实时同步面授班课程到网盘链接（也是授权观看）
你报名后，你的课程顾问会给你发入学通知的邮件；还有一个电子版的合同，等财务审核好后，就以短信的形式发你了，具体进度可以找你的课程顾问老师查一下哈
签完后也记得和我说下，因为只有签完合同后才能开通课程了

需要你登录下课程链接，然后发我你登录的手机号，等签完合同后，我这边给你申请开通课程哈
咱班课程链接（和你入学通知里面的网盘链接一样，你直接保存我发你的）：
通过网盘分享的文件：2025-Linux云计算SRE-N100-新
链接: https://pan.baidu.com/s/1dpeA_ARbQSc0t5pUuoI-xA?pwd=nykc 提取码: nykc 
--来自百度网盘超级会员v7的分享
![[d06ad166684b7ee0328bac2c2d022fc9.png]]


今日分享
确定目标场景是否适合使用SMC加速，是否具备使用SMC加速的条件
https://help.aliyun.com/zh/alinux/user-guide/smc-applicability?spm=a2c4g.11186623.0.0.a73c166c58mR0p
基于eRDMA部署SMC-R和Redis
https://help.aliyun.com/zh/ecs/user-guide/based-on-erdma-enhanced-redis-instance-deployment?spm=a2c4g.11186623.0.i0


每日1问
解释一下dockerfile的ONBUILD指令？


rocky, centos 和 ubuntu
Centos从8开始变成了RHEL的前置版本, Rocky变成了RHEL的开源版本
Ubuntu的内核比较新, 每两年更新一个大版本. 2404 就是24年4月发布的. 一些新的技术比如docker和k8s就需要用到比较新的内核版本. 所以这些年在生产环境用ubuntu的开始多了起来. 而一些老的设备还在跑Centos所以我们也要学习Rocky

自由软件: 代码开源, 软件免费使用. Nginx
共享软件: 代码闭源, 软件月费或者免费使用(广告). 微信这种



如何查看占用或者监听8080端口的进程

```shell
sudo apt install net-tools
netstat -anp | grep 8080 # -a for all, -n for numeric ip, -p for pid
netstat -tulnp | grep 8080
ss -tulnp | grep 8080 # -t for tcp, -u to udp, -l for listen, -n for numeric ip, -p for pid
```

检查服务器开了多久, 查看有没有重启过
```shell
uptime 
```

查看内核版本
```shell
uname -a # ubuntu
Linux ubuntu 6.8.0-88-generic #89-Ubuntu SMP PREEMPT_DYNAMIC Sat Oct 11 00:59:18 UTC 2025 aarch64 aarch64 aarch64 GNU/Linux
```
查看操作系统版本
```shell
xiao@ubuntu:~$ cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.3 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

查看开源软件
```shell
# centos 查看开源软件协议
rpm -qi openssh
rpm -qi kernel
```

查看内存
```shell
lsmem # 查看内存
RANGE                                  SIZE  STATE REMOVABLE BLOCK
0x0000000008000000-0x00000000dfffffff  3.4G online       yes  1-27
0x0000000100000000-0x0000000127ffffff  640M online       yes 32-36
ll /dev/mem # 内存文件
```
查看磁盘
```shell
ll /dev/sda* # 查看所有磁盘
brw-rw---- 1 root disk 8, 0 Dec  1 05:25 /dev/sda
brw-rw---- 1 root disk 8, 1 Dec  1 05:24 /dev/sda1
brw-rw---- 1 root disk 8, 2 Dec  1 05:24 /dev/sda2
```

# 问题
怎么算需要多少台服务器, 比如平时三台服务器, 大概成承受多少请求?
压力测试?
在大促期间, 如何扩充流量
在维护期间, 做容灾测试, 断电, 断网, 等

