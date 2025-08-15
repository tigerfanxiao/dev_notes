Board Gate Protocol 

AD 值
思科: 20 EBGP, 200 IBGP

# 名词解释
### MCE组网： 
在一个CE设备上可能有多个实例。 
场景1： 一些虚拟的运营上可能会租用大运营商的实体设备。 此时对于二级运营商而言， 这台大运营商的CE设备，就可能使它的PE设备。 
场景2： 不同的业务放在不同的实例中

# BGP 和 IGP 的区别
1. BGP 是 AS 之间使用的路由协议. IGP 是 AS 内部使用的路由协议. 
2. BGP 邻居的建立需要 TCP 连接来维持
3. IGP 协议基本是全互通的, 然后通过策略来隔离. BGP是默认是不互通的, 有需求后再配置相通

# BGP邻居建立状态
* EBGP 的邻居建立手动指向的是对端的接口地址而不是 router-id
* IBGP 的邻居建立虽然也可以指向对端的接口地址, 但是我们一般是指向router-id, 因为这样建立TCP-IP连接会比较稳定, 只要 router-id 之间的路由存在, 物理接口的 ip 地址变动, 不会影响 BGP邻居的变动

步骤
1. 建立两个 Peer 之间的 TCP连接
	1. idle 初始状态, 在没有建立邻接之前
	2. Connect 状态 发送建立 TCP 连接的请求
	3. Active 状态, 连接没有完成
2. TCP 连接建立完成后(4 层传输层), 启动 BGP 会话建立(5 层会话层). 发送 Open 报文, 自己进入 OpenSent 状态. 在 Open 消息中包含很多 Capability 参数
3. 收到对端的 Open 报文后, 如果认可 Open 消息中的 Capability 参数, 则进入 OpenConfirm 状态,并发送 keepalive 报文
4. 收到对端发送的 keepalive报文进入 Established 状态, 邻接建立成功
5. 邻居建立成功后, 两遍通过会话, 传递 update 报文(NLRI 字段)来传递前缀更新

报文样例
下图为 Open 报文. 可见在 IP 层后, 是 TCP 协议. TCP 协议之后BGP 的回话. 在Open 报文中包含了 Optional parameters 字段, 里面有 Capability参数. 其中指定了是否是 MP-BGP, 所属的 AS 号等信息
![[Screenshot 2023-04-30 at 12.08.47.jpg]]

下面是 keepalive 消息的报文
![[Screenshot 2023-04-30 at 12.13.14.jpg]]

邻居建立状态查询命令
```shell

debug ip bgp # 查看邻居建立协商过程
show ip bgp neighbors # 查看capability参数

```


### BGP 邻接建立中的 4 种报文
* Open 报文: BGP version, Local AS 号, Hold Time, BGP Router-ID, Capability参数 
> 注意： 如果没有手动配置router-id， BGP会选择最小的环回口作为router-id
> 但是如果没有配置环回口的ip地址， BGP获取不到router-id， 连接建立失败
* Keepalive报文 A message header that keep the Holdtime timer from expiring. keepalive 每个 60 秒发一个. Holdtime 是 180s. 也就是说 180 秒没有收到 keepalive 报文, 就认为邻居挂了
* Update 报文 包含新增或者删除的路由, 路径信息和 NLRI
* Notification Message 各种错误信息


# BGP 选路原则
8 条选路原则
注意: 
* 不同于 OSPF, 带宽不在选路原则里
* 这是 BGP 内部的选路
速记方法: We Love Oranges As Oranges Mean Pure Refreshment
1. Weight   这个特性是思科私有的, 不能传到其他 AS
2. Local Preferance 公用属性, 可以在 AS 中传播, 用于修改出去往哪里走
3. Originate: 本地路由器自己生成的优先于别的邻居传给我的. Local router originate 在 show ip bgp 中next-hop 0.0.0.0. 我通过network 命令, 汇聚,或者重分布搞进来的
4. As Path Length 越短越优先, 多用于修改出去的流量
5. Origin Type 通过 network 命令宣告> EGP 学到的的 > 重分布进来的
6. Multi-Exit Discriminator MED 值越小越好. MED 值在两个 AS 之间传递. 控制inbound流量. MED can be used to advertise to your EBGP neighbors how they should enter your system 
7. Path  EBGP路径优于 IBGP 路径
8. Router ID 越小越优先

EBGP 和 IBGP 邻居
EBGP邻居建立, B 从 A 邻居学到的路由, 会传递给 C
IBGP 邻居之间, B 从 A 邻居学到的路由, 不会传递为 C. 默认是期待在 AS 内部, 所有的 IBGP 邻居形成一种全互联的 mesh 的邻居关系. 也就说, A 的路由要直接传递给剩余的 n-1 个邻居. 但是这种全 mesh 的结构不适合 scale. 所以才有了 Router Reflector 技术. 这个技术解决了两个问题. 
1. 解决了全 mesh 的结构, 而是类似 OSPF 中 DR/BDR 的模式, 构建成星形的结构, 这种方式称为路由反射器RR. 还有一种方式是将 AS 中的设备拆分成多个联邦 Confederation
2. 因为 A 传给 B的路由, B 不能传给 C(水平分割). 有了路由反射器之后, A 直接把路由交给 RR, RR 负责把路由反射给剩下的 n-1个邻居. 
这里出现了一个问题, 就是剩下的 n-1个邻接学到是A 从对面 AS 的路由, 他们自己不是EBGP 的边界路由器, 不知道怎么去往对面的 AS. 这是造成BGP 学到了路由, 但是下一跳不可达的情况. 要解决这个情况, 就需要再边界路由器上告诉自己的IBGP 邻接, 下一跳是自己. 


### BGP 的防环机制
* EBGP 发现有自己的 AS 号, 直接丢弃
* IBGP 水平分割

### BGP 路由黑洞问题
IBGP水平分割规则和BGP可以跨过直连设备建立邻居关系的特点，造成了BGP路由黑洞问题。所谓BGP路由黑洞问题，实际上就是BGP在路由传递过程中跨过中间路由器，因为BGP控制层面和转发层面分离的原因。 在控制层面R4学到了R2发过来的去往R1的路由。 但是在数据转发层面R3因为没有运行BGP， 没有学到去往R1的路由，当数据包道道R3时， 查询路由表失败，导致丢包

![[Pasted image 20230416232210.png]]

1. R1和 R2 是 EBGP 邻居. R1 宣告了一条 1.1.1.1 路由, 传递给 R2
2. R2和 R4 是 IBGP 邻居, R2收到 1.1.1.1 路由后, 传给 R4, 指定下一跳是自己. 
3. 注意: 这里 R3没有跑 BGP, 所以没有学到 1.1.1.1 的路由
4. R4学到1.1.1.1 后又传给了 R5. 
5. 当 R5要访问 1.1.1.1时, 流量走到 R4. R4的查询路由表, 下一跳是R2. 但是因为 R2和 R4没有直连, R4只能查询IGP路由表, 发现要到R2需要经过R3.
6. 流量到达R3时, R3收到包, 查看目的地址是 1.1.1.1, 但是 R3 没有跑 BGP, 没有学到 1.1.1.1的路由, 所以直接丢弃. 此时就产生了路由黑洞

## Multihop 
[参考](https://orhanergun.net/ebgp-multihop#:~:text=BGP%20is%20also%20used%20for,flexibility%20in%20inter%2Ddomain%20routing.)
一般来说 EBGP 只能指定直连的对端设备. Multihop 特性使得 EBGP 可以连接多跳之后的设备. 目的是, 如果原来连接的对端设备挂了, 我们可以与它后面的设备建立 BGP 邻居. 


### Next-hop-self 
为什么会有 next-hop-self. 当边界路由器从 EBGP 邻接那边学到路由, 然后想要传给 iBGP邻居时, 路由的下一跳是对端 EBGP 邻居的地址. 而这个地址 AS 内的IBGP 邻居并不知道. 所以边界路由器就要告诉 iBGP 邻居, 这个路由的下一跳是自己. 经过自己就能到达对端的 AS.

### Peer Group
Peer Group 主要是方便配置用的. 否则我们需要对每一个 iBGP 邻居单独配置 next-hop-self


# IPv6 BGP

1. 手动激活邻居
2. 用route-map手动指定下一跳是自己的接口


# BGP 路由反射器
BGP路由反射器的作用
1. iBGP中使用水平分割来防止环路， 所以在没有路由反射器的情况下， 为了使iBGP邻居都学到彼此的路由， 需要使用全互联的拓扑结构。 BGP路由反射器通过将接受到的路由反射给其他的client 将 iBGP 邻居关系的拓扑从全互联转变为星形结构, 减少连接数量。
2. 可以解决 BGP 路由黑洞问题


如果看到一条路由有Originator 和cluster 两个属性， 说明这条BGP路由时路由反射器的client发过来的
* Originator 是BGP路由反射器的client
* Cluster是路由反射器的ip
* RR能够收下所有vrf传俩的BGP路由， 通过RD来区分
``` shell
show ip bgp vpnv4 all 1.1.1.1  # 从RR 设备上查看客户 1.1.1.1 传来的路由前缀

```
### 反射器的组网方式
反射器一般不走流量， 而是使用旁挂式的组网

# Community 属性
BGP 的 Community 属性可以包含很多信息

在 MPLS VPN 中, Community属性可以包含一下信息
1. RT 值
2. IGP 比如 OSPF 的一些信息, 比如 Area 信息



# BGP Anycast
主要用户DNS或者CDN场景， 可以允许客户访问服务器时找到离自己物理地址最近的节点。 同时当节点宕机时，也能重新计算到新节点的最短路径


