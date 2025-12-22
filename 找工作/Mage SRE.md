
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
Jenkins 课程链接: https://edu.magedu.com/play/1717

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

# ubunt 上显示版本信息
lsb_release -a
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=24.04
DISTRIB_CODENAME=noble
DISTRIB_DESCRIPTION="Ubuntu 24.04.3 LTS"
```

查看开源软件
```shell
# centos 查看开源软件协议
rpm -qi openssh
rpm -qi kernel
```

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
查看终端
```shell
# 查看当前终端
tty
/dev/pts/0
w # 查看当前正在工作的终端


```
查看shell
```shell
# 设备上其实有很多shell
**xiao@ubuntu**:**~**$ cat /etc/shells
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/dash
/usr/bin/screen
/usr/bin/tmux
# 查看当前使用的shell
echo $SHELL
echo ${SHELL}
/bin/bash
```
修改主机名
```shell
hostname # 查看主机名
# 临时修改主机名, 重启失效
hostname es-md-k8s-node1-100-1.xiao.com # 主机名中不能有下划线, 最后为ip地址最后两段
hostnamectl set-hostname <new_hostname> # 永久修改主机名
```

关机重启
```shell
reboot # 重启
shutdown -r now # 立即重启
shutdown -h now # 立即关机
```
查看日历
```shell
sudo apt install ncall
cal 9 2026
```
命令提示符
```shell
echo $PS1
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$

```

内部命令和外部命令
- 外部命令在硬盘上有对应的文件
# 问题
怎么算需要多少台服务器, 比如平时三台服务器, 大概成承受多少请求?
压力测试?
在大促期间, 如何扩充流量
在维护期间, 做容灾测试, 断电, 断网, 等

# 招工作
【大表哥内推1】福建朴朴信息技术有限公司，福建福州 30K * 15薪 ，本科
岗位名称 高级运维工程师(基础网络)  @所有人  
（实际薪资会浮动，根据职级来定）
岗位职责
1、负责制定和落实公有云,数据中心资源的管理流程和规范,推动运维标准化和自动化,以保证运维的服务质量与交付效率;
2、负责基础设施的故障,风险管理,设计和周期性组织基基础设施的故障演练,保证基础设施的可用性,容灾性,以及故障时的快速主动恢复能力;
3、负责主机,网络,云资源等基础设施的持续规划和演进,持续使是升稳定性,安全性,以支撑业务持续发展；
4、负责合理规划团队目标与执行上级交办事项,为团队成员提供技术指旨导与支持,提升团队影响力。
任职要求
1、精通Linux/Unix系统管理,网络协议(TCP/IP、HTTP等),运维工具(Ansible、Terraform等),Shell/Python/Go等任意编程语言;
2、熟悉基础设施可观测性体系的建设(如Prometheus,ELK)；
3、具备主流云平台(国内外各大公有云)及其上各项云资源管理经验,具备数据中心网络规划,服务器资源规划,虚拟化平台规划的经验;
4、具备良好的项目管理能力,制定计划,把控进度,协调资源,确保项目交付;
5、具备良好的故障排查能力和抗压能力,能够快速响应和处理多发事件；
6、具备良好的沟通能力和团队协作精神,有较强的文档编写能力。

【大表哥内推2】四川融科智联科技有限公司；

地点：新疆（干满两年也可以回成都）
工资待遇：15-20K
中间件运维工程师
任职资格：
1、本科及以上学历（能力强的专科也可以）
2、精通一门运维开发语言，不仅限于python, golang, java等；
3、深入理解并掌握Linux、Docker、Kubernetes、spring cloud等开源系统组件的架构及实现原理，具备容器、微服务等技术的二次开发与上层应用部署能力；
4、熟悉常见服务的应用运维，以及常见问题和参数调优。包括：nginx、MySQL、Redis、ElasticSearch、Spark、Prometheus、grafana、Zabbix、Zookeeper、RocketMQ、Nacos、RabbitMQ、Kafka、eurka、fastdfs、infuxdb、skywalking等；
5、熟悉 DevOps 理念及相关生态，具备3年以上大规模高可用分布式系统集群的实践经验；
6、具备较强的问题分析和解决能力，务实主动且有良好的沟通协作能力；
7、有较强学习能力，具有良好的团队协作精神，具备管理能力者优先，能适应出差。

# 学习方法
1. 在一段时间内, 把一个目标击穿. 一个学习的topic(Simon 学习法). 击穿之后, 达到一个阶段. 后面所有的新知识会挂在老的知识上. 一段时间内只有一个学习目标. 不要在不同的分支中切换, 浪费宝贵的思考时间. 比如在6个月内学习考试K8s直到拿到Kubestranaut 证书
2. 要学会取舍, 要把有限的经历用来击穿某一个学科, 千万不要全面出击. 只有不容易管理, 降低效率
3. 一个课程要学习三遍. 
	1. 第一遍研究课程主要内容, 书本的目录, 行业的综述. 了解自己学习的大致方向, 范围, 初步规划学习的总体周期. 
	2. 第二遍进入细节的知识点进行学习, 联系, 逐个击破每个知识点. 如果判断把一个知识点学好了. 要用费曼学习法. 把知识点讲出来. 如果讲不出来则表示知识点没有吃牢, 如果能讲出来则表示知识点已经吃牢
	3. 第三部, 把尽可能多的知识点串联起来, 从整体上去记忆理解知识点. 并和其他相关知识体系进行融合
4. 目前不再是跟着马哥的一个课程走, 而是按照Topic走. 每段时间只关注一个Topic, 知道把这个topic相关的内容就理解记忆完成. 最后在整体做项目的时候, 查看哪个topic还需要加码
5. 看过的视频内容不要想着过一段时间再会看, 要在第一次看的时候, 尽量搞懂要学习的内容, 做好笔记
6. 固定和有计划的安排学习时间. 因为要学习和做实验的内容太多了, 加上工作忙碌, 很容易就忘记自己走到哪里了
7. 在这个文件中把所有找工作需要用的技术栈的内容整理出来, 链接到对应的文档上去. 还要分析技术章的成熟度
8. 设定每天的学习目标

### 找工作
1. 证书
	1. K8s
	2. AWS
### Soft Skill 技能栈
1. Business Sence
	1. 站在管理者的角度. 用工成本的和收益, 最好的人, 最合适的人, 最搞性价比的人, 取决于不同的岗位吗?
	2. 人员的培训, 人员的潜力, 人员的服从性, 人员的自主意识. 人员的发展路径Carrer Path
	3. 人员的薪资, 人员的激励, 人力的成本, 服务质量和成本的控制. 收益
	4. 工程师的价值. 降低成本, 提高效率
	5. 技术的选型, 软件包的供应链
2. 项目管理
3. 
### 专业技能栈分析
1. 精通一门运维开发语言
	1. Python
	2. Go
	3. Java
	4. NodeJS
2. Linux
	1. [[Ngnix]]
3. Devops [[DevOps]]
	1. [[Git]] 
	2. CI/CD
		1. [[Jenkins]]
	3. Docker
		1. [[Docker Basics]]

4. [[Project Management]]
	1. 项目真实案例