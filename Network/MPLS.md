
### MPLS 的本质是什么?
本质上是一种隧道技术, 这种隧道也称为 LSP Label Switch Path

### 什么隧道, 谁建立的隧道?
所谓隧道, 在数据转发的时候, 它不再需要像普通的 IP 路由那样在每一跳路由器上都剥掉二层帧头, 基于 IP 地址, 通过查询路由表来进行选路. 在 MPLS 域中, 通过标签分发协议(一般情况下是 LDP)来建立起一条 LSP, 数据直接通过标签的交换就能到达 MPLS 域另一端的 PE 设备

### 标签是怎么形成的?
标签是由 LDP 协议分发完成的

### 什么控制层面和数据转发层面
MPLS是一个很好的控制层面和转发层面分离的例子. 
一般来说, 控制层面指的是路由前缀的传递. 比如两个 PE 设备的 VPNv4路由的传递. P设备之间基于 IGP 协议的路由前缀的传递. 
数据转发层面则描述了数据传递过程中, 数据包真正的行为. 换句话说, 因为两个层面的分离, 即使在控制层面, 两台 PE 设备之间已经完成了路由前缀的传递, 但是也可能因为转发层面的问题, 造成数据无法传递. BGP 的路由黑洞问题就是一个很好的例子. BGP 的路由黑洞问题简单的说, MPLS 域中的三台设备, 两台 PE 设备之间建立的 iBGP 链接, 在控制层面传递了路由. 但是因为中间的P 设备没有运行 BGP 协议, 没有学到 BGP 路由. 数据在到达 P 设备后, 查询路由表无果, 进而将数据包丢弃, 形成路由黑洞. 


### 为什么有次末跳弹出机制 PHP
Penulitimate Hop Popping 为了避免在LSP 的最末跳PE设备上进行 2 次查询. 可以设想在MPLS 域的数据转发层面, 数据通过标签交换转发. 如果没有 PHP 机制, 数据在到达最后一跳时, 首先要查询 LFIB, 发现目的路由前缀为直连路由, 然后进行 MPLS 标签的弹出. 暴露出原有的 IP 路由前缀, 然后查询 FIB, 找到出接口. 这个过程经历了两次查询. 
有 PHP 之后, 数据在次末跳的时候, 就 pop MPLS 标签. 然后直接把IP 数据包直接从出接口扔出去. 注意我认为从转发层面来看, 不需要做 IP 路由寻址. 当数据导到最末跳设备时, 因为是 IP 包, 所以直接查询 FIB表, 进行 IP 路由寻址, 到达目的网段. 换句话说最末跳设备只需要做一次查询. 从而减少了 PE 设备的负载. 
	在 MPLS 域中, PE 设备往往要创建多个 VRF, 运行 MP-BGP协议, 承载客户网络的路由前缀, 所以负载很大. 这也是为什么开发了 PHP 机制的原因

### 在 MPLS 协议中, 有哪些表参与了
RIB 路由信息库: 最基本的路由表
FIB 转发信息库: 基于思科的特快转发(Cisco Express Forwarding)为了实现快速转发, 不需要每次都递归查询 RIB 表. 就做了 FIB 表, 这个也是 IP 包到达路由器真正在查询的表
LIB 标签信息库: 存放自己产生的和我收到的为某一条路由前缀捆绑的标签. 由 LDP 协议生成        
LFIB: 由 FIB和 LIB 组成的表. 入栈标签, 出栈标签, 下一跳, 出接口

# LDP
LDP 协议是一种标签分发协议. 它由两个作用
1. 为本机上所有 IGP 协议的路由前缀生成一个标签
2. 通过建立 LDP 邻居, 将本机生成的标签分发给自己的 LDP 邻居

* 基于 LDP 的机制, 我们将本机基于 IGP 前缀生成的标签称为 Input 标签, 而把从邻居收到的标签称为Output标签. 
* 我们本地生成的标签, 永远是给别人用的. 换句话说, 我前面的邻居, 会剥掉它收到数据帧中的 LDP 报头, 压上我给它的 LDP 标签, 然后把新的数据帧发给我. 

# MPLS的由来

在已经有了 IP 网络的情况下, 为什么要做标签转发?
首先我们来看 IP 路由的过程. 当一组电信号来到路由器器, 我们首先在二层成帧. 在帧的头部查看目的mac 地址是否时自己接口的 mac 地址, 如果不是, 则将帧丢弃, 如果是, 则拆开帧头查看三层的 IP 包头. 
查看 IP 包头中的源 IP 和目的 IP, 如果目的 IP 是自己, 则进一步查看 IP 包的内容, 进入四层分析. 如果 IP 目的地址不是自己, 则查询路由表. 找到下一跳地址和出接口, 然后将 IP 包重新用以太帧封装, 将二层帧的目的地址换做下一跳的 mac 地址, 源地址换成自己的出接口的 mac 地址. 这种经过一个节点就更换帧头的行为, 称为解封装. 但是, 这个时候, 如果我们没有下一跳的 mac 地址, 我们还得发送 ARP 广播, 从而知道下一跳IP 地址和 mac 地址的对应关系. 
我们总结一下做了哪些特点
1. IP 路由是逐跳的. 每一跳都要重写二层信息, 源目的 mac
2. 既要做二额层交换又要做三层转发. 

为什么一般认为二层交换比三层转发要快? 
假设我们是交换机, 我们收到一个帧, 我查看一下目的mac 地址, 查询 ARP 表, 然后就从接口扔出去了. 它不需要重写二层头部, 甚至不需要做解封装. 更不需要做路由表递归查询. 而且厂家已经通过硬件来做
二层除了以太网技术还有 PPP 和帧中继 FR 

MPLS 的含义
* Multi-Protocol, 指支持 IPv4 和 IPv6, 可以承载组播和单播
* Label Switching 在 MPLS 域内, 我的数据是依据标签转发

MPLS 特征
* 标签对应的是目的地址的路由前缀. 
* MPLS 依赖 IP 路由即 CEF 交换. 因为标签是基于路由生成的. 需要打开 ip cef
注: CEF Cisco Express Forwarding 快速转发表. 保存在内存中的 routing table
NSF No Stop Forwarding: makes the routing information maintained by CEF available to the backup route procesor.

# MP-BGP
在 MPLS 网络中, 我们通过 VRF 来接受和隔离不同客户的内网路由. 这里我们面对两个问题: 
1. 接受. 需要一种协议能够承载大量的路由条目.  - BGP
2. 隔离. 需要一种方法来隔离不同客户内网路由.   RD 值
基于上面两个需求, 我们扩展了原生的 BGP 协议. 即Multi-Protocol BGP 多协议的 BGP 协议. MP-BGP 协议首先是一种 BGP 协议, 他们可以承载海量的路由条目. 同时在支持 IPv4的基础上, 还支持
* IPv6
* VPNv4
* 标签分发
* 组播

传统的 BGP, 也称为 BGP-4, 是在RFC1771中定义的. 用户承载IPv4前缀. 而MP-BGP 是在 RFC2858 中定义的


### 为什么在 MPLS 中要使用 MP-BGP
我们从控制层面和数据转发层面两个角度来看
* 首先看控制层面
在 MPLS 域的边界 PE 设备上, 因为要建立多个 VRF 来接受不同客户站点的路由, 且这些路由最终需要通过主设备的上 MP-BGP 协议来通告给MPLS 域另一端的 PE 设备. 因为客户不同站点的路由信息, 可能存在重复的情况(一般是客户内网 IP ), 所以通过在路由前缀的前面加上 RD (Route Distinguisher)值来区分. 此时 MP-BGP 协议传递的不再是IPv4的路由前缀, 而是被称为 VPNv4 的路由前缀, 长度为 96 位. 结构为`|RD| IPv4 prefix|` 

因为 PE 设备建立的多个 VRF, 所以从 VRF 中学到的 BGP 路由是有 vrf 地址族在管理的. 我们需要将 vrf 地址族中的前缀放到 vpnv4 的地址族中, 然后当这些前缀到达 MPLS 域对端的 PE 设备时, 又要把这些前缀从 vpnv4地址族中取出来, 放到不同的 vrf 地址族中. 所以就需要设置 RT Router Target值. RT值需要配置两个参数, 即 import 和 export. 

举个例子, 左侧的客户 A 通过 MPLS 域给另一侧的客户 A 站点传递路由前缀. 在把vrf 地址族中路由前缀放到 vpnv4 地址族时, 加上 RT 值 Export 标签 123. 对端设备配置 vrf A 的 RT Import 值为 123. 也就说, 它会从所有的 vpnv4地址族的路由前缀中, 把RT值为 123的前缀挑出来, 引入自己的 VRF A 中. 

![[Pasted image 20230430123741.png]]

RD 值和 RT 值的传播是基于 BGP Community 属性完成的. 
RD 值的一般格式 `AS号:唯一标识`

信息查看命令

```shell
show ip bgp vpnv4  # 查看vpnv4 路由
show ip bgp vpnv4 rd x:y labels # 查看为 vpnv4路由分配的标签
show ip bgp vpnv4 all summary # 查看 vpnv4的邻居
debug ip bgp vpnv4 unicast updates

```

# MPLS中的 RR 设备

PE 设备默认过滤 RT 值行为
MPLS 域中, PE 设备的默认行为是过滤掉所有的 VPNv4前缀, 除非, 这些前缀中有 RT 值, 可以导入 VRF 的路由表中
RR 设备不会过滤 VPNv4的前缀. 但是这种情况对 RR 的性能是一种挑战. 在现网架构中, 往往是双 RR, 两台 RR 分别反射不同的RT 值. 如下图: RR1 用来反射RT 值为偶数的前缀, RR2 用来反射 RT 值为奇数的前缀
```shell
no bgp default route-target filter # 关闭默认 RT 值过滤行为, 所有的路由都会放进设备.
```

![[Pasted image 20230501180404.png]]

```shell
router bgp 1
address-family vpnv4
bgp rr-group 1
ip extcommunity-list 1 permit rt 1:1
ip extcommunity-list 1 permit rt 1:3
```

BGP多路径负载
* eBGP
* iBGP
* eiBGP 一条通过 eBGP 邻居收到的, 一条通过 iBGP 邻居收到的

```shell
router bgp 1
address-family vpnv4
maximum-path ebgp 4
```

如果在两台不同的 PE 设备上, 用相同的 RD 值创建 VRF.  RR 收到两条相同的路由前缀. 默认情况下, RR 会根据 13 条选路规则进行PK, 然后只反射Best前缀. 如果要想要反射两条路由, 建议选用不同的 RD 值. 以防止其中一台 PE 设备挂掉的时候, RR 不必等到收敛后才将新路由前缀反射出去. 


# 重分布
重分布在 vrf 地址组下做, 而不是 vpnv4 地址族下做



# VRF
子接口也可以划分到 VRF
子接口单臂路由实验



# MPLS 和 Segment Routing 的关系

1. Segment Routing本质上是 MPLS 2.0
	1. MPLS用的 LDP 来做标签分发, Segment Routing 用扩展的 IGP 协议来做标签分发. 这样就省去了 LDP 协议, 减轻了控制层的负担
	2. 在数据层面没有改动


