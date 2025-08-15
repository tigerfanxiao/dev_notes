[参考视频](https://www.bilibili.com/video/BV1wB4y1W7na?p=9&vd_source=3d7d2ddd3035c9f21a34739d4a0a4eb8)
云计算学完之后里面不包含云原生的 docker 和 k8s的内容， 但这是devops的必修内容

云计算的5个基本特征


# VPC
* VPC 使用来做网路隔离的. 两个 VPC 之间天然是不通的, 需要做对等连接, AWS 中称为 VPC peer
* 在一个 VPC 内部可以划分多个子网. 子网间天然是可以互通的. 如果需要进行隔离, 可以使用安全组的概念. 也称为 Security Group
* 每个 VPC 都会自动创建一个虚拟的路由器. 用来连接外网. 
* 创建 ECS 的时候, 可以配置多张网卡, 连接到已经创建好的 VPC 中的某个子网上. 可以选择 DHCP 自动获取子网中的 IP 地址. 
* 如果允许外部远程访问, 就需要购买 EIP, 然后用 SSH 访问

### Security Group
默认的安全组:
* Webserver 把 80, 443 端口都打开了
* 远程访问 把 22, 3389 mstsc 访问打开了

# 服务器硬件知识
做Raid 磁盘整列, 在物理机内是需要一张 Raid 卡的

# 容灾方案
* 一般认为两地三中心的方案是最高等级的容灾方案. 
* 不要把两个 VDC (virtual DC)放在一个 AZ 里

现在常见的服务器CPU 的架构有 X86 架构, ARM架构

### 云计算是基于虚拟化技术. 
虚拟化技术的对象: 计算资源, 存储资源, 网络资源. 
常见的虚拟化软件 hypervisor/VMM有: 
VMware Vsphere + Esxi
Huawei FusionCompute = VRM + CNA
微软: Hyper-V
Redhat KVM
思杰 XEN

如果说一个 CPU 有 32 个 core, 一个 core 可以由两个线程. 一个线程可以虚拟一个 vcpu, 那么一台双 CPU 的物理服务器, 可以虚拟出 128 个 vCPU

一般创建一个 ECS 会给 40G 的系统盘. 这里 40G 的系统盘里面有 23 个 G 是操作系统用的, 也称为系统母盘, 它是和另外 127 个 ECS 共享的. 然后剩下的 17G 是每个 ECS 独占的, 也称为差分盘, 是需要另外收费的.  估计是转各自的应用或者服务. 


### 云计算的3中服务模式
IaaS， PaaS， SaaS
IaaS 包含虚拟出来的 CPU ECS, 存储, 网络资源和操作系统， 对象化存储
PaaS 面向应用开发者程序员需要使用的开发环境, 比如 python, jdk, 数据库等
SaaS 就是应用软件作为服务了 
DaaS 如果平台积累了海量的数据, 把数据分析的结果拿出来卖, 就是 DaaS. 比如阿里卖尿布的购买量来推测这些孩子未来几年的消费趋势

### 云架构

SC 运营平台(就是页面上配置和卖服务的地方) +  OC运维平面
云内核 + 服务: FusionSphere Openstack + 服务
虚拟化系统: FusionCompute 把物理资源虚拟化
底层: 物理设备

### 云计算的4中部署模式
1. 私有云 企业对数据的安全性要求很高的场景
2. 公有云
3. 社区/行业云 比如整个城市警察用的监控系统, 远程教育系统, 医院的系统
4. 混合云: 公有云, 私有云一起用


# Openstack
nova 接受计算资源
cinder 接受存储资源
neutron 接受网络资源
ironic 接受非虚拟化的裸金属设备BMS



# 存储
华为的分布式存储 Fusion Storage = FSM + FSA 是Master和agent架构
华为的集群计算 Fusion Compute = VRM + CNA
可以用一些老旧的服务器作为存储的控制节点
一套funsion storage可以管理 3到4096台服务器， 管理49K的硬盘。 构建一个资源池。 

硬盘框 scale up 有上限， 在实际环境中 96块ssd， HHD 128块。 就会造成性能下降
1 U = 4.445 cm
高密度硬盘框里面可以放75块盘
硬盘也分为3.5英寸， 和2.5英寸
所有的硬盘框都有级联模块（2个作为冗余）， 级联模块中用 PRI和EXP， 作为冗余

存储的设备分为控制设备和存储设备
1. 控制框比如OceanStor 5300V3 盘控一体， 自带25块硬盘。 如果用完就链接硬盘框
链接原则 ：
1. A---A, B---B
2. EXP-PRI, PRI-EXP

# 网络
云计算的DC以东西向流量会很大（服务器和服务器之间的流量）， 传统的DC是南北向流量（客户端和服务器）
因为虚拟机热迁移的时候， 会产生大量的横向流量


HCNP学习内容
Linux
OpenStack

day 1 学到了 P8


